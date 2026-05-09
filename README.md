# streaming-01-foundations

[![Python 3.14](https://img.shields.io/badge/python-3.14%2B-blue?logo=python)](./pyproject.toml)
[![MIT](https://img.shields.io/badge/license-see%20LICENSE-yellow.svg)](./LICENSE)

> Streaming data analytics: local streaming foundations.

Data analytics requires a variety of skills.
This course builds capabilities through working projects.

In the age of generative AI, **durable skills** are grounded in real work:
setting up a professional environment,
reading and running code,
understanding the logic,
and pushing work to a shared repository.
Each project follows the structure of professional Python projects.
We learn by doing.

## This Project

This project introduces the workflow shape used throughout the course.

The project does not require Kafka to run,
but we start the install process
and begin practicing with multiple terminals.

Installations are early to allow for issues.

Our producer this week reads sales records from a local CSV file,
processes each record one at a time,
and writes consumed records to an output CSV (it's a proxy for a
Kafka topic that we will use for "real" streaming projects).

Kafka setup begins here,
but Kafka does not need to be running for this project to succeed.
The goal is to get the local project working first and
begin work with Kafka so we can use it in the next module.

Ask lots of questions - we are here to help.
It's only really bad the very first time we use it.
It gets better.

## Working Files

You'll work with just these areas:

- **data/** - input data and generated output files
- **docs/** - the project narrative and documentation
- **src/streaming/** - producer, consumer, and supporting code
- **pyproject.toml** - update authorship & links
- **zensical.toml** - update authorship & links

## Instructions

Follow the
[step-by-step workflow guide](https://denisecase.github.io/pro-analytics-02/workflow-b-apply-example-project/)
to complete:

1. Phase 1. **Start & Run**
2. Phase 2. **Change Authorship**
3. Phase 3. **Read & Understand**
4. Phase 4. **Modify**
5. Phase 5. **Apply**

## Challenges

Challenges are expected.
Sometimes instructions may not quite match your operating system.
When issues occur, share screenshots, error messages, and details about what you tried.
Working through issues is part of implementing professional projects.

## Success

After completing Phase 1. **Start & Run**, you'll have your own GitHub project
running with Kafka.

Use four named terminals for practice:

1. **kafka** - where kafka will run (if Win, use WSL)
2. **topics** - manage topics (if Win, use WSL)
3. **producer** - run the project and producer
4. **consumer** - run the consumer

After the producer and consumer run successfully, you should see:

```shell
========================
Consumer executed successfully!
========================
```

A new file `project.log` will appear in the root project folder and
the producer will stream messages to a new **data/output** file.
The consumer will read and process message events from that file.

## Command Reference

The commands below are used in the workflow guide above.
They are provided here for convenience.

Follow the guide for the **full instructions**.

<details>
<summary>Show command reference</summary>

### In a machine terminal (open in your `Repos` folder)

After you get a copy of this repo in your own GitHub account,
open a machine terminal in your `Repos` folder:

```shell
# Replace username with YOUR GitHub username.
git clone https://github.com/username/streaming-01-foundations

cd streaming-01-foundations
code .
```

### In VS Code Terminal 1: Start Kafka (kafka)

For full instructions see
[start kafka](https://denisecase.github.io/pro-analytics-02/kafka/start-kafka/).

If any command fails,
repeat the steps at
[install kafka](https://denisecase.github.io/pro-analytics-02/kafka/install-kafka/)
until starting up is reliable.

Open a terminal (if Windows, use **WSL**)
and run the commands one at a time.
Rename this terminal to `kafka`.

Step 1. Verify Java and PATH

```bash
echo "$JAVA_HOME"

"$JAVA_HOME/bin/java" --version
```

Step 2. Rebuild ClusterID (as needed)

```bash
cd ~/kafka

rm -rf /tmp/kraft-combined-logs

KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"

echo "Cluster ID: $KAFKA_CLUSTER_ID"

bin/kafka-storage.sh format --standalone -t "$KAFKA_CLUSTER_ID" -c config/server.properties
```

Step 3. Start kafka server (keep running)

```bash
cd ~/kafka

bin/kafka-server-start.sh config/server.properties
```

### In VS Code terminal 2: Create Topic (topics)

For full instructions see
[create topic](https://denisecase.github.io/pro-analytics-02/kafka/create-topic/).

The topic name must match the name given in your
`.env` file.
Copy `.env.example` to `.env` to create it.

Open another terminal (if Windows, use **WSL**)
and run the commands one at a time.
Rename this terminal to `topics`.

```bash
cd ~/kafka

bin/kafka-topics.sh --create \
  --bootstrap-server localhost:9092 \
  --partitions 1 \
  --replication-factor 1 \
  --topic streaming-01-foundations-case
```

### In VS Code Terminal 3: Run Project and Producer (producer)

Open another terminal (if Windows, use **Powershell**)
and run the commands one at a time.
Rename this terminal to `producer`.

```shell

```shell
# reset uv cache only after suspected cache corruption or strange dependency errors
# uv cache clean

uv self update
uv python pin 3.14
uv sync --extra dev --extra docs --upgrade

uvx pre-commit install

git add -A
uvx pre-commit run --all-files

# repeat if changes were made by pre-commit tasks
git add -A
uvx pre-commit run --all-files

# run the producer (produces messages)
uv run python -m streaming.producer_case

# do chores
uv run python -m ruff format .
uv run python -m ruff check . --fix
uv run python -m pyright
uv run python -m pytest
uv run python -m zensical build

# save progress
git add -A
git commit -m "your message here"

# repeat if changes were made (try the UP ARROW)
git add -A
git commit -m "your message here"

git push -u origin main
```

### In VS Code Terminal 4: Run Consumer (consumer)

Open another terminal (if Windows, use **Powershell**)
and run the commands one at a time
and start the consumer.
Rename this terminal to `consumer`.

```shell
clear
uv run python -m streaming.consumer_case
```

</details>

## Notes

- Use the **UP ARROW** and **DOWN ARROW** in the terminal to scroll through past commands.
- Use `CTRL+f` to find (and replace) text within a file.
- You do not need to add to or modify `tests/`. They are provided for example only.
- Many files are silent helpers. Explore as you like, but nothing is required.
- You do NOT not to understand everything; understanding builds naturally over time.

## Troubleshooting >>> or

If you see something like this in your terminal: `>>>` or `...`
You accidentally started Python interactive mode.
It happens.
Press `Ctrl+c` (both keys together) or `Ctrl+Z` then `Enter` on Windows.

## Missing .env?

See [create topic](https://denisecase.github.io/pro-analytics-02/kafka/create-topic/)
for why we must copy `.env.example` to `.env`.

## Many Terminals

See [many terminals](https://denisecase.github.io/pro-analytics-02/kafka/many-terminals/)
for how we name our terminals (and if Windows, how we get the different types).
You can split terminals shown below, or just click between them as you like.

## Example Producer Output

```text
| INFO | P01 |   Sending local message with key=CA-QC
| INFO | P01 |   MESSAGE SENT  sent=3
| INFO | P01 | ========================
| INFO | P01 | SECTION E. Exit
| INFO | P01 | ========================
| INFO | P01 | Summary:
| INFO | P01 |   Sent 3 message(s).
| INFO | P01 | WROTE TOPIC_CSV = data\output\streaming-01-foundations-case.csv
| INFO | P01 | ========================
| INFO | P01 | Producer executed successfully!
| INFO | P01 | ========================
```

## Example Consumer Output

```text
| INFO | C01 | MESSAGE CONSUMED
| INFO | C01 | consumed=3
| INFO | C01 | No new message received within 10.0s timeout.
| INFO | C01 | Producer finished or paused. Stopping consumer.
| INFO | C01 | Saving artifacts...
| INFO | C01 | WROTE OUTPUT_CSV = data\output\consumed_sales.csv
| INFO | C01 | ========================
| INFO | C01 | SECTION E. Exit
| INFO | C01 | ========================
| INFO | C01 | Summary:
| INFO | C01 |   Consumed 3 message(s).
| INFO | C01 | ========================
| INFO | C01 | Consumer executed successfully!
| INFO | C01 | ========================
```
