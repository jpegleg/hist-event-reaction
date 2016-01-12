# hist-event-reaction
A sec.pl based history file event correlation template.

Read the Simple Event Correlator documentation: https://simple-evcorr.github.io/

Run the following script to configure and launch a sec.pl daemon that monitors all /home/*/.bash_history and /root/.bash_history and writes it to a central log, timestamping it when it receives the data from the file.

hist-event-reaction-configuration

To enable this bash history monitoring to timestamp and process in real time, apply the follow to the user:


export HISTCONTROL=ignoredups:erasedups
shopt -s histappend
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$'\n'}history -a; history -c; history -r

This centralized log creates sec.pl contexts which can be used for further rule processing. For example, you can create a rule for the context "rm -rf /tmp/stuff" and it will be able to execute futher actions like running scripts or creating more contexts.

Example rule expansion version of hist-event-reaction.conf that looks for "whatevercommand" and then runs /usr/local/bin/evaluationprogram when "whatevercommand" is read:


type=Single
ptype=RegExp
desc=Reading history input $0 
pattern=(.*?)
action=create $0

type=Single
ptype=RegExp
desc=Reading for whatevercommand pattern
pattern=whatevercommand
action=spawn /usr/local/bin/evaluationprogram

As it currently is, you will probably see the entire history file dumped on logon and exit. This is still useful to see, but it would be much more clean to just have the live append and not the full dump. I will probably be leaving it this way by default but I might update this readme on how to avoid the history file dumps.
