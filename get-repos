#!/bin/bash

prev=$PWD

repos=$(find ~ -name ".git" \
    | grep -Ev "(.local|.cache)" \
  | sed "s/\.git//g")

for repo in $repos; do
  cd $repo

  curr_repo=$(echo $repo \
      | sed "s/\/data\/data\/com.termux\/files\/home\///" \
    | sed "s/\/home\/$USER\///")

  echo -e "\e[94mRepo\e[m:"$curr_repo

  # check for gh remote
  if ! git fetch origin &> /dev/null; then
    echo -e "\x1b[91mNo github remote!\x1b[m"

    echo "Delete repo? [d]"
    echo "Create github remote [c]"
    echo "Ignore [*]"

    read -n 1 -r -s
    if [[ $REPLY =~ ^[d]$ ]]
    then
      echo -e "\x1b[91mAre you sure?\x1b[m [y/n*]"
      read -n 1 -r -s
      if [[ $REPLY =~ ^[y]$ ]]
      then
        rm -rf $repo
        echo -e "\x1b[91mRemoved\x1b[m $curr_repo"
      else
        echo "Canceled deletion of $curr_repo"
      fi
    elif [[ $REPLY =~ ^[c]$ ]]
    then
      gh repo create $curr_repo -s . --push --public
    fi

    # check for unstaged files
  elif git status | grep -w "Changes not staged" &> /dev/null; then
    echo -e "\x1b[93mUnstaged changes\x1b[m"

    echo -e "Open \x1b[95mlazygit\x1b[m? [y/n*]"
    read -n 1 -r -s
    if [[ $REPLY =~ ^[y]$ ]]
    then
      lazygit
    fi

    # check for untracked files
  elif git status | grep -w "Untracked files" &> /dev/null; then
    echo -e "\x1b[93mUntracked files\x1b[m"

    echo -e "Open \x1b[95mlazygit\x1b[m? [y/n*]"
    read -n 1 -r -s
    if [[ $REPLY =~ ^[y]$ ]]
    then
      lazygit
    fi

    # check if files need to be committed
  elif git status | grep -w "Changes to be committed:" &> /dev/null; then
    echo -e "\x1b[96mFiles need to be committed\x1b[m"

    echo -e "Open \x1b[95mlazygit\x1b[m? [y/n*]"
    read -n 1 -r -s
    if [[ $REPLY =~ ^[y]$ ]]
    then
      lazygit
    fi

    # check if pull needed
  elif git status | grep -w "git pull" &> /dev/null; then
    git pull && echo -e "\x1b[94mFiles pulled from remote\x1b[m"
    # check if clean
  else
    echo -e "\x1b[92mUp to date\x1b[m"
  fi
done

cd $prev
