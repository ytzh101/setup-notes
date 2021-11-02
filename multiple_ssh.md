# Create Multiple SSH keys for Multipe Git accounts 

## Create 2 ssh keys for 2 git accounts. 
$ ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -C "abc@gmail.com"

$ ssh-keygen -t rsa -f ~/.ssh/id_rsa -C "xyz@work.com"

## 2 Key files and corresponding .pub files 
~/.ssh/id_ed25519 

~/.ssh/id_rsa   

# Ensure the ssh-agent is running. 
Either use the "Auto-launching the ssh-agent", or start it manually:

## start the ssh-agent in the background
$ eval "$(ssh-agent -s)"
> Agent pid 59566

## Auto-launching ssh-agent on Git for Windows
You can run ssh-agent automatically when you open bash or Git shell. Copy the following lines and paste them into your ~/.profile or ~/.bashrc file in Git shell:

####################################################

env=~/.ssh/agent.env

agent_load_env() { test-f "$env"&& . "$env">| /dev/null ; }

agent_start() {
    (umask077; ssh-agent >| "$env")
    . "$env">| /dev/null ; }

agent_load_env

#agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo$?)
if[ ! "$SSH_AUTH_SOCK"] || [ $agent_run_state= 2 ]; then
    agent_start
    ssh-add
elif[ "$SSH_AUTH_SOCK"] && [ $agent_run_state= 1 ]; then
    ssh-add
fi

unsetenv

##########################################################
