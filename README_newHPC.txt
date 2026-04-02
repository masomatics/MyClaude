# Setup on a new HPC / machine

1. git clone git@github.com:masomatics/MyClaude.git ~/MyClaude
2. mkdir -p ~/.claude
3. ln -s ~/MyClaude/global_CLAUDE.md ~/.claude/CLAUDE.md

# First-time git identity (if not already set)
git config --global user.name "masomatics"
git config --global user.email "koyama.masanori@gmail.com"

# To update Claude instructions from any machine:
# - Edit ~/MyClaude/global_CLAUDE.md
# - git add -A && git commit -m "update" && git push
# On other machines: git pull (symlink picks it up automatically)
