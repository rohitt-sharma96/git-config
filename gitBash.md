
my_git() {
  GIT_BRANCH=$(git branch --all 2> /dev/null | egrep "^\*" | cut -d ' ' -f 2 )
  if [[ -z "$GIT_BRANCH" ]]; then
      echo "" #not in a Git repo
  else
      # Check for Untracked files
      if [ $(git status | egrep "^Untracked" -c) -ge 1 ]; then
          # ANSI code: Red
          echo -e "(\033[0;31m$GIT_BRANCH\033[0m) "
      # Check for Changes (Modified/Staged)
      elif [ $(git status | egrep "^Changes" -c) -ge 1 ]; then
          # ANSI code: Yellow
          echo -e "(\033[0;33m$GIT_BRANCH\033[0m) "
      else
          # ANSI code: Green
          echo -e "(\033[0;32m$GIT_BRANCH\033[0m) "
      fi
  fi
}

# PS1 Updated:
# \W use kiya hai current folder ke liye
# \u@\h hata diya hai taaki simple dikhe, agar chahiye toh wapis laga sakte hain
export PS1="\[\033[32m\]➜ \[\033[36m\]\W \[\033[0m\]\$(my_git)"

# Change direcotry aliases
alias home='cd ~'
alias cd..='cd ..'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'


# change into the old directory
alias bd='cd "$OLDPWD"'
