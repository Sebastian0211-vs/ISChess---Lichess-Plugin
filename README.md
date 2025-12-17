# ISChess-UCI

**Lichess Bot Plugin for ISChess**

ISChess-UCI is an **optional extension (DLC)** for the ISChess project.
It allows any ISChess bot to run **as a real chess engine on Lichess**, using the standard **UCI protocol**.

No modification of the ISChess core logic is required.

---

## 1. Prerequisites

Before using this plugin, you must already have:

* A working **ISChess** installation
* A chess bot implemented in `Bots/`
* Python **3.10 or newer**
* Git

---

## 2. Using your own bot

### 2.1 Create your bot

Add a new file in the ISChess project:

```text
Bots/MyBot.py
```

Your bot must register itself using `register_chess_bot(...)`.
All bots placed in `Bots/` are **automatically loaded** when ISChess-UCI starts.

No change to `ISChess_uci.py` is required.

---

### 2.2 Selecting the bot (no code modification)

Bot selection is done **at launch time**, not by editing source files.

The bot name must match the name passed to `register_chess_bot("MyBot", ...)`.

Example:

```yaml
engine_options:
  bot: "MyBot"
```

---

## 3. Clone the Lichess bot client

ISChess-UCI relies on the official Lichess bot client.

Clone it **next to** your ISChess project:

```bash
git clone https://github.com/lichess-bot-devs/lichess-bot.git
cd lichess-bot
```

Create and activate a virtual environment (strongly recommended):

### Windows

```powershell
python -m venv .venv
.\.venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## 4. Create a Lichess bot account

1. Go to [https://lichess.org](https://lichess.org)
2. Create a **new account** (do not use your personal one)
3. Do *not* play a single game as a Human

---

## 5. Generate a Lichess API token

1. Go to [https://lichess.org/account/oauth/token](https://lichess.org/account/oauth/token)
2. Create a new token with:

   * `bot:play` permission
3. Copy the token (you will not see it again)

---

## 6. Configure ISChess-UCI for Lichess

Inside the `lichess-bot` folder, create or edit:

```text
config.yml
```

### Example configuration (ISChess-UCI)

```yaml
token: "API_KEY"
url: "https://lichess.org/"

engine:
  protocol: "uci"

  # Path to ISChess project (relative to lichess-bot/)
  dir: "../ISChess"
  working_dir: "../ISChess"

  # UCI entry point
  name: "ISChess_uci.py"

  
  interpreter: "python"  # path to venv python.exe from ISChess

  # Select your bot (must be registered in Bots/)
  engine_options:
    bot: "Gambit"

  ponder: false
  debug: true
  silence_stderr: false

challenge:
  concurrency: 1
  accept_bot: true
  only_bot: false
  variants:
    - standard
  time_controls:
    - blitz
    - rapid
  modes:
    - rated

matchmaking:
  allow_matchmaking: true
  challenge_variant: "standard"
  challenge_mode: "rated"
  challenge_timeout: 1
  challenge_initial_time:
    - 180
    - 300
  challenge_increment:
    - 2
    - 3
```

---

## 7. Running the bot on Lichess

Make sure the virtual environment is active, then from the `lichess-bot` directory:

```bash
python lichess-bot.py
```

### 7.1 Request bot status

First launch will ask you to turn your account into a Bot account run this command : 

```bash
python lichess-bot.py -u
```

If everything is correct:

* ISChess-UCI starts as a UCI engine
* The selected bot is loaded automatically
* Lichess connects and starts games

Your ISChess bot is now **playing online**.

---

## 8. Summary

* ISChess-UCI is a **deployment layer**
* Bots are selected via configuration, not code
* Any bot in `Bots/` can be deployed
* Lichess-bot handles matchmaking and networking
