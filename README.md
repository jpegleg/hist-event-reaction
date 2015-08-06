# hist-event-reaction
A sec.pl based history file event correlation template.

Run the following script to configure and launch a sec.pl daemon that monitors all /home/*/.bash_history and /root/.bash_history and writes it to a central log, timestamping it when it receives the data from the file.

To enable this bash history monitoring to timestamp and process in real time, apply the follow to the user:


export HISTCONTROL=ignoredups:erasedups
shopt -s histappend
export PROMPT_COMMAND="${PROMPT_COMMAND:+$PROMPT_COMMAND$'\n'}history -a; history -c; history -r"

This centralized log creates sec.pl contexts which can be used for further rule processing. For example, you can create a rule for the context "rm -rf /tmp/stuff" and it will be able to execute futher actions like running scripts or creating more contexts.

Example rule expasion version of hist-event-reaction.conf:

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
