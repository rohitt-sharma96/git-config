



```bash
# ----- COLORS -----
GREEN="\[\033[32m\]"
CYAN="\[\033[36m\]"
RED="\[\033[31m\]"
YELLOW="\[\033[33m\]"
RESET="\[\033[0m\]"

# ----- GIT BRANCH -----
parse_git_branch() {
  git rev-parse --abbrev-ref HEAD 2>/dev/null
}

# ----- GIT DIRTY CHECK -----
is_git_dirty() {
  git diff --quiet --ignore-submodules HEAD 2>/dev/null
  [ $? -ne 0 ] && echo " ✗"
}

# ----- PROMPT -----
build_prompt() {
  local branch=$(parse_git_branch)
  local dirty=$(is_git_dirty)

  PS1="${GREEN}➜ ${CYAN}\W"

  if [ -n "$branch" ]; then
    PS1="${PS1} ${GREEN}git:${RED}(${branch})"
    if [ -n "$dirty" ]; then
      PS1="${PS1}${RED}${dirty}"
    fi
  fi

  PS1="${PS1}${RESET} "
}

PROMPT_COMMAND=build_prompt



```