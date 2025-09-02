📝 Jenkins Pipeline Notes (Clear & Simple)
1. agent

Tells Jenkins where to run the job.

Examples:

agent any → run on any available node.

agent { label 'linux' } → run only on nodes with label "linux".

2. stages & steps

stage = big block of work (like Build, Test, Deploy).

steps = actual commands inside a stage.

Example:

stage('Build') {
    steps { echo "Compiling code" }
}

3. environment

Used to define environment variables.

Example:

environment {
    APP = "myapp"
    VERSION = "1.0"
}


Use with $APP or ${VERSION}.

4. post

Runs after a stage or pipeline finishes.

Types:

always {} → runs always.

success {} → runs only if success.

failure {} → runs only if failure.

changed {} → runs if build status changed.

5. deleteDir()

Cleans the workspace folder (removes all files).

Used for cleanup before/after builds.

6. options

Set rules/limits for the pipeline.

Examples:

timeout(time: 10, unit: 'MINUTES') → stop build after 10 mins.

disableConcurrentBuilds() → don’t run two builds at the same time.

timestamps() → add time to logs.

7. parameters

User inputs asked before job starts.

Examples:

parameters {
    string(name: 'VERSION', defaultValue: '1.0')
    choice(name: 'ENV', choices: ['dev', 'qa', 'prod'])
}

8. input

Pauses the pipeline during execution to ask approval or input.

Examples:

input message: "Deploy to production?", ok: "Yes, Deploy"


or with options:

input(
  message: "Choose environment",
  parameters: [choice(name: 'ENV', choices: ['dev','qa','prod'])]
)


✅ Quick memory trick:

agent = where to run

stage/steps = what to do

environment = variables to use

post = what happens after

deleteDir() = clean workspace

options = rules for pipeline

parameters = ask before run

input = ask during run