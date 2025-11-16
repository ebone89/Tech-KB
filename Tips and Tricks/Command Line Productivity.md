# Command Line Productivity

Tips and tricks to boost your command-line productivity.

## ⌨️ Keyboard Shortcuts

### Linux/Mac Terminal
```
Ctrl+A          Move to beginning of line
Ctrl+E          Move to end of line
Ctrl+U          Delete to beginning of line
Ctrl+K          Delete to end of line
Ctrl+W          Delete previous word
Ctrl+L          Clear screen
Ctrl+R          Search command history
Ctrl+C          Cancel current command
Ctrl+Z          Suspend current process
Ctrl+D          Exit shell

Alt+B           Move back one word
Alt+F           Move forward one word
Alt+.           Insert last argument of previous command
```

### Command History
```bash
# History navigation
↑ / ↓           Navigate history
Ctrl+R          Reverse search history
Ctrl+S          Forward search (if enabled)

# History commands
history         Show command history
!123            Execute command #123
!!              Execute last command
!$              Last argument of last command
!*              All arguments of last command
!^              First argument of last command

# Search history
history | grep "keyword"
```

## 🔄 Aliases and Functions

### Useful Aliases
```bash
# Add to ~/.bashrc or ~/.zshrc

# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias ~='cd ~'
alias -- -='cd -'

# List files
alias ll='ls -lah'
alias la='ls -A'
alias l='ls -CF'
alias lt='ls -lth'          # Sort by time
alias lS='ls -lSh'          # Sort by size

# Safety nets
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias mkdir='mkdir -p'

# Git shortcuts
alias g='git'
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git pull'
alias gd='git diff'
alias gco='git checkout'
alias gb='git branch'
alias glog='git log --oneline --graph'

# Docker shortcuts
alias d='docker'
alias dc='docker-compose'
alias dps='docker ps'
alias di='docker images'

# System
alias update='sudo apt update && sudo apt upgrade'  # Debian/Ubuntu
alias ports='netstat -tuln'
alias psg='ps aux | grep -v grep | grep -i -e VSZ -e'

# Quick edits
alias bashrc='vim ~/.bashrc'
alias vimrc='vim ~/.vimrc'

# Network
alias myip='curl ifconfig.me'
alias speedtest='curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python -'
```

### Useful Functions
```bash
# Extract any archive
extract() {
    if [ -f $1 ]; then
        case $1 in
            *.tar.bz2)   tar xjf $1     ;;
            *.tar.gz)    tar xzf $1     ;;
            *.bz2)       bunzip2 $1     ;;
            *.rar)       unrar e $1     ;;
            *.gz)        gunzip $1      ;;
            *.tar)       tar xf $1      ;;
            *.tbz2)      tar xjf $1     ;;
            *.tgz)       tar xzf $1     ;;
            *.zip)       unzip $1       ;;
            *.Z)         uncompress $1  ;;
            *.7z)        7z x $1        ;;
            *)           echo "'$1' cannot be extracted via extract()" ;;
        esac
    else
        echo "'$1' is not a valid file"
    fi
}

# Create directory and cd into it
mkcd() {
    mkdir -p "$1" && cd "$1"
}

# Find process by name
psgrep() {
    ps aux | grep -v grep | grep -i -e VSZ -e "$@"
}

# Quick backup
backup() {
    cp "$1"{,.bak}
}

# Calculator
calc() {
    python3 -c "print($*)"
}

# Weather
weather() {
    curl wttr.in/$1
}
```

## 🔍 Command Line Navigation

### Directory Shortcuts
```bash
# Use pushd/popd for directory stack
pushd /path/to/dir      # Push and cd
popd                    # Return to previous dir
dirs -v                 # View directory stack

# Jump back multiple directories
cd ../..                # Up 2 levels
cd ../../..             # Up 3 levels

# Return to previous directory
cd -
```

### Auto-completion
```bash
# Tab completion
command [TAB]           # Complete command
path/to/f[TAB]          # Complete file/directory

# Double tab for options
git [TAB][TAB]          # Show all git commands
```

## 🚀 Productivity Tools

### fzf - Fuzzy Finder
```bash
# Install
sudo apt install fzf    # Ubuntu/Debian
brew install fzf        # macOS

# Usage
Ctrl+R                  # Fuzzy search history
Ctrl+T                  # Fuzzy search files
Alt+C                   # Fuzzy search directories

# In commands
vim $(fzf)              # Open file in vim
cd $(find . -type d | fzf)  # Fuzzy cd
```

### tmux - Terminal Multiplexer
```bash
# Sessions
tmux                    # New session
tmux new -s name        # Named session
tmux ls                 # List sessions
tmux attach -t name     # Attach to session
tmux kill-session -t name

# Inside tmux (prefix = Ctrl+b)
Ctrl+b %               # Split vertically
Ctrl+b "               # Split horizontally
Ctrl+b arrow           # Switch pane
Ctrl+b c               # New window
Ctrl+b n/p             # Next/previous window
Ctrl+b d               # Detach
```

### screen - Alternative Multiplexer
```bash
# Sessions
screen                  # New session
screen -S name          # Named session
screen -ls              # List sessions
screen -r name          # Reattach
screen -d -r name       # Detach and reattach

# Inside screen (prefix = Ctrl+a)
Ctrl+a c               # New window
Ctrl+a n/p             # Next/previous window
Ctrl+a d               # Detach
Ctrl+a S               # Split horizontal
Ctrl+a |               # Split vertical
Ctrl+a Tab             # Switch region
```

## 📝 Command Line Tricks

### One-Liners
```bash
# Find and replace in multiple files
find . -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Find largest files
find . -type f -exec du -h {} + | sort -rh | head -10

# Monitor file changes
watch -n 5 'ls -lh file.txt'

# Count files by extension
find . -type f | sed 's/.*\.//' | sort | uniq -c | sort -rn

# Remove duplicate lines while preserving order
awk '!seen[$0]++' file.txt

# Convert all text files to Unix format
find . -name "*.txt" -exec dos2unix {} \;

# Find broken symbolic links
find -L /path -type l

# Disk usage summary
du -sh */ | sort -rh

# Monitor logs in real-time
tail -f /var/log/syslog | grep --line-buffered "error"

# Quick HTTP server
python3 -m http.server 8000
```

### Redirection Tricks
```bash
# Redirect stdout and stderr
command > output.txt 2>&1
command &> output.txt       # Bash shorthand

# Append to file
command >> file.txt

# Redirect stderr only
command 2> errors.txt

# Discard output
command > /dev/null 2>&1

# Tee - output to both file and screen
command | tee output.txt
command | tee -a output.txt  # Append

# Here document
cat << EOF > file.txt
Line 1
Line 2
EOF

# Here string
grep "pattern" <<< "string to search"
```

### Process Substitution
```bash
# Compare output of two commands
diff <(ls dir1) <(ls dir2)

# Use output as input file
command --file <(other_command)

# Multiple inputs
paste <(cat file1) <(cat file2)
```

## ⚡ Performance Optimization

### Command Optimization
```bash
# Use built-ins over external commands
# Faster: [[ -f file ]]
# Slower: test -f file

# Avoid useless use of cat
# Good: grep pattern file.txt
# Bad: cat file.txt | grep pattern

# Use xargs for parallel processing
find . -name "*.jpg" | xargs -P 4 -I {} convert {} {}.png

# Faster find alternatives
# Use locate for known files
locate filename

# Use fd (faster find)
fd pattern
```

### Disk I/O
```bash
# Show I/O statistics
iostat -x 1

# Monitor disk usage
iotop

# Show file system cache usage
vmstat 1
```

## 🎯 Environment Customization

### Prompt Customization
```bash
# Add to ~/.bashrc

# Simple colored prompt
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# Git-aware prompt
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PS1='\u@\h:\w $(parse_git_branch)\$ '
```

### Environment Variables
```bash
# Add to ~/.bashrc or ~/.zshrc

# Custom PATH
export PATH="$HOME/bin:$PATH"
export PATH="/usr/local/bin:$PATH"

# Editor
export EDITOR=vim
export VISUAL=vim

# History settings
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTCONTROL=ignoredups:erasedups
shopt -s histappend  # Append to history, don't overwrite

# Colors
export CLICOLOR=1
export LS_COLORS='di=34:ln=35:so=32:pi=33:ex=31:bd=34;46:cd=34;43:su=30;41:sg=30;46:tw=30;42:ow=30;43'
```

## 💡 Pro Tips

1. **Use `!!` to repeat last command with sudo**:
   ```bash
   apt update          # Oops, forgot sudo
   sudo !!             # Runs: sudo apt update
   ```

2. **Quick backup before editing**:
   ```bash
   cp config.txt{,.bak}  # Creates config.txt.bak
   ```

3. **Run command on multiple files**:
   ```bash
   chmod +x script{1,2,3}.sh  # Expands to script1.sh script2.sh script3.sh
   ```

4. **Brace expansion for sequences**:
   ```bash
   echo file{1..10}.txt      # file1.txt file2.txt ... file10.txt
   mkdir -p project/{src,bin,docs,tests}
   ```

5. **Use `Ctrl+X Ctrl+E` to edit command in editor**:
   - Opens current command in your default editor
   - Useful for complex multi-line commands

6. **Background and foreground jobs**:
   ```bash
   long_running_command &    # Run in background
   jobs                      # List jobs
   fg %1                     # Bring job 1 to foreground
   bg %1                     # Resume job 1 in background
   ```

## Related Topics

- [[Linux/Common Commands|Linux Commands]]
- [[Linux/Bash Scripting Basics|Bash Scripting]]
- [[Tips and Tricks Index|Tips and Tricks]]

---

#productivity #commandline #tips #linux #terminal
