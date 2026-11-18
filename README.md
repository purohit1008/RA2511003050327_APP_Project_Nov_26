# Doodles

Doodles is a real-time multiplayer drawing and guessing game written in Java (Swing). Players join a
room, take turns drawing a secret word, and the others guess it in the chat. The project contains an
authoritative TCP game server, desktop clients, a MySQL persistence layer and the SQL schema.

The project files are inside the `Doodles/` folder in this GitHub repository. The paths below are written from the repository root.

## Course and team details

### Course

| Course ID | Course name |
| --- | --- |
| 21CSC203P | Advanced Programming Practice |

### Team members

| Full name | Register number | Department | Section | Academic year |
| --- | --- | --- | --- | --- |
| Mahesh Rajpurohit (Team Leader) | RA2511003050327 | CSE | F | 2026–2027 |
| Mohit Katal | RA2511003050343 | CSE | F | 2026–2027 |
| Ujjwal Singh Chauhan | RA2511003050546 | CSE | F | 2026–2027 |

Academic year above refers to the current academic session, 2026–2027.

### Mentor

| Mentor full name | Designation |
| --- | --- |
| Dr. S. Shargunam | Assistant Professor |

## GitHub submission details

- **Repository name:** `RA2511003050327_APP_Project_Nov_26` (matches the required naming pattern).
- **Subject category:** APP.
- **Final project report:** No report PDF was present in the supplied archive. Add the final report
  as a clearly named PDF (for example, `docs/APP_Project_Report_Nov_2026.pdf`).
- **GitHub actions still required:** Invite all teammates and
  `SRMIST-TRC-CSE-STUDENT-PROJECTS` as collaborators, make sure the invitations are accepted, and
  obtain at least five repository stars. These actions must be completed on GitHub; they cannot be
  completed by editing this archive.

## Features

- **Rooms:** quick play, public rooms, private rooms with a 5-character code, lobby with host controls, host transfer if the host leaves
- **Turns:** 2-8 players; the drawer changes after every turn; the host chooses the number of rounds (1-10) and the drawing time (30-180 s)
- **Words:** 700+ words in 17 categories; the drawer picks from 3 choices; progressive hints during the round
- **Scoring:** speed-based points, drawer bonus, a scoreboard for each round plus an overall leaderboard next to the chat, podium at the end
- **Chat:** guesses and chat with 32 emoji, 14 stickers and custom messages
- **Power-ups (unique feature):** players spend points to attack others or help themselves
  - Drawer: Blur, Flip, Spotlight
  - Guesser: Freeze, Hint, Double, Shield
- **Drawing tools:** 9 brushes, eraser, fill, line, rectangle, ellipse, undo, clear, colour picker, save the drawing as PNG
- **Persistence:** players, game sessions, drawings and chat messages are stored in MySQL

## Project modules

| Folder | Purpose |
| --- | --- |
| `Doodles/src/client` | Game window and screen, drawing canvas, brushes, tool dock |
| `Doodles/src/server` | TCP server: rooms, rounds, word selection, scoring, power-up validation |
| `Doodles/src/common` | Shared classes: `Config` (host/port), players, messages, power-ups, emotes |
| `Doodles/src/ui` | Welcome screen, lobby, room browser, dialogs, leaderboard, power shop, theme, icons |
| `Doodles/src/database` | JDBC connection and data access objects (players, sessions, drawings, chat) |
| `Doodles/database` | `doodles_database.sql`, the MySQL schema |
| `Doodles/lib` | `mysql-connector-j-26.10.0.jar`, the JDBC driver |
| `Doodles/screenshots` | Project output |

## Requirements

- JDK 21 or newer
- MySQL Server 8.0 or newer, running on `localhost:3306`
- MySQL Connector/J 26.10.0: already included in `Doodles/lib/` (also declared in `Doodles/pom.xml` for Maven users)

## Database setup

1. Start MySQL Server.
2. Run the schema script (`Doodles/database/doodles_database.sql`) with MySQL Workbench or:
   ```sh
   cd Doodles && mysql -u root -p < database/doodles_database.sql
   ```
   **Warning:** the script starts with `DROP DATABASE IF EXISTS doodles_db`. It deletes and recreates that database and inserts sample rows, so only run it on a database you can reset.
3. The server connects as MySQL user `root` with password `Doodles@123` by default. If your MySQL account is different, do not edit the source. Set the values instead, using either environment variables or Java options on the **server**:

   | Setting | Environment variable | Java option |
   | --- | --- | --- |
   | URL | `DOODLES_DB_URL` | `-Ddoodles.db.url=...` |
   | User | `DOODLES_DB_USER` | `-Ddoodles.db.user=...` |
   | Password | `DOODLES_DB_PASSWORD` | `-Ddoodles.db.password=...` |

When the server starts it checks the database and prints `Database: READY` (or says exactly what is wrong).
If MySQL is unavailable the game still runs, but nothing is saved.

## How to run

Start the **server first**, then one **client per player**. The default port is `5051`.

### Option A: run scripts (macOS / Linux / Windows)

```sh
cd Doodles
./run-server.sh        # or run-server.bat on Windows
./run-client.sh        # or run-client.bat on Windows, once per player
```

The scripts compile the project and use the driver from `lib/`.

### Option B: IntelliJ IDEA

Open the `Doodles/` project folder, make sure `lib/mysql-connector-j-26.10.0.jar` is added as a library of the module, then run:

1. `server.GameServer`
2. `ui.WelcomeScreen` (run it once per player; allow multiple instances in the run configuration if needed)

### Option C: Maven

```sh
cd Doodles
mvn clean compile
mvn dependency:build-classpath -Dmdep.outputFile=target/classpath.txt
java -cp "target/classes:$(cat target/classpath.txt)" server.GameServer
java -cp "target/classes:$(cat target/classpath.txt)" ui.WelcomeScreen
```

(On Windows PowerShell use `;` instead of `:` in the classpath.)

### Playing on two computers

Start the clients with `-Ddoodles.host=<IP address of the server computer>`. To change the port, use
`-Ddoodles.port=NNNN` on the server and on every client.

## Troubleshooting

| Message | Fix |
| --- | --- |
| Could not connect to the Doodles server | Start `GameServer` first |
| No answer from the server | Another program uses that port; change it with `-Ddoodles.port` on server and clients |
| `port 5051 is already in use` | An old server is still running; stop it |
| MySQL driver is not on the classpath | Make sure the jar is in `lib/` and added as a library |
| wrong MySQL user or password | Use the table in "Database setup" step 3 |
| database / table is missing | Run `Doodles/database/doodles_database.sql` |

## Screenshots

| Welcome screen | Lobby |
| --- | --- |
| ![Welcome screen](Doodles/screenshots/welcome-screen.jpg) | ![Lobby with players](Doodles/screenshots/lobby-room-players.jpg) |

| Word choice | Active drawing round |
| --- | --- |
| ![Word choice](Doodles/screenshots/word-choice.jpg) | ![Active drawing round](Doodles/screenshots/active-drawing-round.jpg) |

| Guessing view (blurred by a power-up) | Power-up targeting |
| --- | --- |
| ![Blurred guessing view](Doodles/screenshots/blurred-guessing-view.jpg) | ![Power-up target](Doodles/screenshots/power-up-target.jpg) |

| Round results |
| --- |
| ![Round results](Doodles/screenshots/round-results.jpg) |
