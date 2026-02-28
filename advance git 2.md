
   ``` normal folder  {OUTPUT}  - ➜ folderName         GIT ❌```

  ``` folder with git init {OUTPUT} - ➜ folderName git:(main)  GIT ✅```

```bash

# ----- COLORS -----
GREEN="\[\033[32m\]"
CYAN="\[\033[36m\]"
RED="\[\033[31m\]"
RESET="\[\033[0m\]"

# ----- GIT BRANCH LOGIC (Handles 'HEAD' issue) -----
get_git_branch() {
  # Pehle check karte hain ki branch name kya hai
  local branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)
  
  # Agar naya repo hai (git init ke baad), branch name 'HEAD' dikhata hai
  # Use theek karke 'main' ya 'master' dikhane ke liye:
  if [ "$branch" == "HEAD" ]; then
    branch=$(git symbolic-ref --short HEAD 2>/dev/null || echo "main")
  fi
  
  echo "$branch"
}

# ----- PROMPT FUNCTION -----
build_prompt() {
  # 1. Base prompt: ➜ folder_name
  PS1="${GREEN}➜ ${CYAN}\W${RESET}"

  # 2. Check if .git folder exists ONLY in the current directory
  if [ -d ".git" ]; then
    local branch=$(get_git_branch)
    
    # 3. Check if dirty (✗ symbol)
    local dirty=""
    if ! git diff --quiet --ignore-submodules HEAD 2>/dev/null; then
      dirty=" ✗"
    fi

    # 4. Git info append karein agar branch exist karti ho
    if [ -n "$branch" ]; then
      PS1="${PS1} ${GREEN}git:${RED}(${branch})${RED}${dirty}${RESET}"
    fi
  fi

  # 5. Final space for typing
  PS1="${PS1} "
}

# Command run karne ke liye
PROMPT_COMMAND=build_prompt


```
