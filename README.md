[Fedora]("https://fedoraproject.org/w/uploads/2/2d/Logo_fedoralogo.png") 
# Fedora-configurations
Some simple tools and scripts that make using the Fedora distribution easier.

# **Source global definitions**
```bash
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi
```
# **User specific environment with check don't duplicate paths**
```bash
add_path() {
[ -d "$1" ] && case ":$PATH:" in
*":$1:"*) ;;
*) export PATH="$1:$PATH" ;;
esac
}
```
## for use
```bash
add_path "$HOME/.local/bin"
```

```bash
unset -f add_path
```
# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions
```bash
if [ -d ~/.bashrc.d ]; then
for rc in ~/.bashrc.d/*; do
[ -f "$rc" ] && . "$rc"
done
fi
unset rc
```
# **Aliases**
```bash
alias up="sudo dnf upgrade --refresh"
alias cl="clear"
alias pyp="source $HOME/.venv/bin/activate"
alias ll="eza -ah --icons --group-directories-first"
alias ..="cd .."
alias ...="cd ../../"
alias cp="cp -iv"
alias rm="rm -rf"
alias e="exit"
alias h="hx"
alias htmux="tmux split-window -h -p 75 'hx .'"
alias n="nvim"
alias v="vim"
alias ncdu="sudo ncdu -e -t 4 -2 --exclude-kernfs"
# alias scrcpy="scrcpyc --tcpip -S -m"
alias scrcpy="scrcpyc --keyboard=uhid --mouse=uhid --no-playback 2>/dev/null"
alias glow="glow -l -t -a"
```
# **Search dnf command clean & modern**
```bash
dnfsearch() {
if [ -z "$1" ]; then
echo "Usage: dnfsearch <package_name>"
return 1
fi

# ANSI color codes
local GREEN='\033[1;32m'
local NC='\033[0m' # No Color / Reset

# Retrieve all installed package names into a lookup array
local -A installed_map
while read -r pkg; do
[[ -n $pkg ]] && installed_map["$pkg"]=1
done < <(dnf list --installed 2>/dev/null | awk 'NR>1 {print $1}')

# Search packages, filter out header noise, and check installed status
dnf search --name "$1" 2>/dev/null | awk '/^\s*(=|Matched)/ {next} {print $1}' | while read -r pkg; do
[[ -z $pkg ]] && continue
if [[ -n ${installed_map[$pkg]} ]]; then
echo -e "$pkg ${GREEN}[Installed]${NC}"
else
echo "$pkg"
fi
done
}
```
## for used
```bash
dnfsearch <package_name>
# output ex: dnfsearch btop
# ~  dnfsearch btop
# btop.x86_64 [Installed]
# texlive-bibtopic.noarch
# texlive-bibtopicprefix.noarch
# usbtop.x86_64
```
