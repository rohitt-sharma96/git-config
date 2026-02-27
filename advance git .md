# STEP - 1

GO to nano ~/.bashrc

## STEP-2

Paste below code in that file

```bash
# ----- COLORS -----
GREEN="\[\033[32m\]"
CYAN="\[\033[36m\]"
RED="\[\033[31m\]"
RESET="\[\033[0m\]"

# ----- PROMPT FUNCTION -----
build_prompt() {
  # Base prompt: ➜ project
  PS1="${GREEN}➜ ${CYAN}\W${RESET}"

  # Check if .git folder exists ONLY in the current directory
  if [ -d ".git" ]; then
    local branch=$(git rev-parse --abbrev-ref HEAD 2>/dev/null)
    
    # Check if dirty
    local dirty=""
    if ! git diff --quiet --ignore-submodules HEAD 2>/dev/null; then
      dirty=" ✗"
    fi

    # Append git info to PS1
    if [ -n "$branch" ]; then
      PS1="${PS1} ${GREEN}git:${RED}(${branch})${RED}${dirty}${RESET}"
    fi
  fi

  # Add the final space at the end
  PS1="${PS1} "
}

PROMPT_COMMAND=build_prompt
```

## STEP - 3

run in git bash-> exec bash

summary:- current folder dikhayega
         git initialize hoga to hi uska branch dikhayega nhi to nahi dikhayega
         X cross bhi dikhayega if file is (modified,stage,untracked)

       OUTPUT - ➜ webfllyx                 agar nahi hai to  
                ➜ webflyx git:(add_classics) agar git initialize hai to  
