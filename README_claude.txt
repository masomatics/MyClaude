========================================
CLAUDE'S NOTES ON PATH HANDLING ISSUES
========================================

## HOW TO DETERMINE $HOME (the root directory)

$HOME is the user's home directory. To find it, run: echo ~
All paths in this file use $HOME to refer to this root directory.
In shell commands, both $HOME and ~ expand to the same path.

🚨🚨🚨 EXIT CODE 2 ERROR - IMMEDIATE ACTION REQUIRED 🚨🚨🚨

If you EVER see an error like:
  "Exit code 2"
  "No such file or directory"
  "can't open file"

STOP IMMEDIATELY. DO NOT retry the same command again.

This means the working directory is WRONG. You MUST:
1. Re-read this README_claude.txt file
2. Use the wrapper script: ~/${projname}_cmd.sh <command>
   OR use cd prefix: cd $HOME/${projname} && <command>
3. NEVER run the same bare command again without fixing the path

⚠️⚠️⚠️ CRITICAL: THE WRAPPER MUST BE IN THE COMMAND STRING ITSELF ⚠️⚠️⚠️

A common failure mode is SAYING you will use the wrapper in your description
but NOT actually including it in the command parameter. This is WRONG:

  WRONG: Bash tool call with:
    description: "Run with wrapper"
    command: "python script.py"        ← WRAPPER IS MISSING FROM COMMAND!

  CORRECT: Bash tool call with:
    description: "Run with wrapper"
    command: "~/fastslow_video_cmd.sh python script.py"  ← WRAPPER IS IN COMMAND

The wrapper/cd prefix must appear IN THE COMMAND STRING, not just in your intent.

Example of WRONG behavior (DO NOT DO THIS):
  ✗ python script.py          → Exit code 2
  ✗ python script.py          → Exit code 2 (retrying same mistake!)
  ✗ python script.py          → Exit code 2 (still wrong!)

Example of CORRECT behavior:
  ✗ python script.py          → Exit code 2
  ✓ ~/fastslow_video_cmd.sh python script.py  → SUCCESS
  ✓ cd $HOME/fastslow_video && python script.py  → SUCCESS

🚨🚨🚨 END EXIT CODE 2 SECTION 🚨🚨🚨

⚠️ FIRST ACTION AFTER READING THIS FILE:
Check for and read $HOME/${projname}/log_claude.txt
(See "SESSION LOG" section below for details)

⚠️⚠️⚠️ OUTPUT FILES - ALWAYS PROVIDE PATHS ⚠️⚠️⚠️

Whenever you produce a small number of image/animation files (up to 10 PNGs/GIFs),
ALWAYS provide the user with the ABSOLUTE path to each file.

⚠️⚠️⚠️ END OUTPUT FILES ⚠️⚠️⚠️

⚠️⚠️⚠️ PERMISSIONS - NEVER ASK FOR READ OPERATIONS ⚠️⚠️⚠️

YOU HAVE 100% PERMISSION TO READ ANY FILES WITHOUT ASKING.

This includes:
✓ find - NEVER ask for permission
✓ grep - NEVER ask for permission
✓ ls - NEVER ask for permission
✓ cat / head / tail - NEVER ask for permission
✓ Read tool on ANY file - NEVER ask for permission
✓ Glob tool for ANY pattern - NEVER ask for permission
✓ Grep tool for ANY search - NEVER ask for permission
✓ ANY read-only operations - NEVER ask for permission

NEVER EVER ask for permission for read operations. Just do it.
If you ask for permission to use find/grep/ls/read, you are doing it WRONG.

CRITICAL: Use the Read tool, Glob tool, and Grep tool directly instead of
wrapping grep/find/ls in bash commands. These tools don't trigger approval prompts.

⚠️⚠️⚠️ END PERMISSIONS ⚠️⚠️⚠️

## HOW TO DETERMINE ${projname}

${projname} is the project directory name under $HOME/

To find ${projname}:
1. Check the current working directory: `pwd`
   - If output is $HOME/fastslow_video, then ${projname} = fastslow_video
   - If output is $HOME/akorn, then ${projname} = akorn
   - If output is $HOME/fastslow, then ${projname} = fastslow
2. Or check the git repository root: `git rev-parse --show-toplevel`
   - Extract the directory name after $HOME/
3. The ${projname} is the directory name immediately under $HOME/

Example: If working in $HOME/fastslow_video, then ${projname} = fastslow_video

## SETUP INSTRUCTIONS - CREATE THE WRAPPER SCRIPT

When starting work in a project, create the ${projname}_cmd.sh wrapper script:

1. Create `$HOME/${projname}_cmd.sh` with the following content:
   ```bash
   #!/bin/bash
   cd $HOME/${projname}
   exec "$@"
   ```

2. Make it executable:
   ```bash
   chmod +x $HOME/${projname}_cmd.sh
   ```

## SESSION LOG - log_claude.txt

At the START of every session, check for log_claude.txt in the working directory:

**Location:** $HOME/${projname}/log_claude.txt

**Rules:**
1. If log_claude.txt EXISTS → READ IT FIRST to understand what happened in previous sessions
2. If log_claude.txt DOES NOT EXIST → CREATE IT

**Purpose:**
- Maintains continuity between Claude sessions
- Documents progress, decisions, and issues encountered
- Helps future sessions understand context without re-explaining

**What to log:**
- Key tasks completed
- Important decisions made
- Errors encountered and how they were resolved
- Current state/progress of ongoing work
- Any context the next session would need

🚨🚨🚨 MANDATORY AUTOMATIC LOGGING — NON-NEGOTIABLE 🚨🚨🚨

You MUST update log_claude.txt IMMEDIATELY and AUTOMATICALLY after:
- ANY code change (file created, modified, or deleted)
- ANY experiment/test run (log numerical results, metrics, findings)
- ANY important decision or discovery

DO NOT wait for the user to ask. DO NOT wait until "later".
DO NOT batch multiple changes before logging.
Log EACH significant action AS SOON AS IT HAPPENS.

This is NOT optional. Failing to log automatically is a FAILURE MODE.
The user should NEVER have to remind you to log.

⚠️ WHAT TO LOG (with examples) ⚠️

1. CODE CHANGES: What file, what was changed, why
   Example: "Modified dynamic_klayer.py: added spectral norm to cond_connectivity"

2. EXPERIMENT RESULTS: Full numerical results table immediately after run
   Example: "Ran diagnostic --mnist: add final_loss=0.0147, concat=0.0158,
   concat coupling norm exploded to 73k vs add 12k"

3. FINDINGS & INSIGHTS: Any non-obvious observation from results
   Example: "Coupling norm ratio (concat/add) is 6.2x at production scale
   vs 2x at synthetic scale — scale-dependent amplification"

4. ERRORS: Full error messages and how resolved

⚠️ TIMING RULES ⚠️

- After running a script/test → log results in the SAME response
- After editing code → log what changed in the SAME response
- After a discovery → log the insight in the SAME response
- NEVER say "I'll log this later" — there is no later

The log is the institutional memory. If it's not logged, it didn't happen.

**Example workflow at session start:**
```bash
# Check if log exists
~/${projname}_cmd.sh ls -la log_claude.txt

# If it exists, read it (or use Read tool directly on $HOME/${projname}/log_claude.txt)
# If it doesn't exist, create it with initial entry
```

⚠️⚠️⚠️ MANDATORY RULE - READ THIS FIRST ⚠️⚠️⚠️

⭐⭐⭐ **OPTION 1: USE THE WRAPPER SCRIPT (PRIMARY METHOD - ALWAYS TRY THIS FIRST)** ⭐⭐⭐

THIS IS THE DEFAULT. USE THIS FOR EVERY BASH COMMAND UNLESS IT FAILS.

Use ~/${projname}_cmd.sh to automatically handle directory changes:

✓ BEST:    ~/${projname}_cmd.sh python tasks/rl/utils/re_evaluate_models.py
✓ BEST:    ~/${projname}_cmd.sh git status
✓ BEST:    ~/${projname}_cmd.sh ls -la

The ${projname}_cmd.sh script:
- Should exist at $HOME/${projname}_cmd.sh
- Should be executable (chmod +x already done)
- Automatically cd's to $HOME/${projname} before running commands
- Sources the correct virtual environment if needed

WHY USE THE WRAPPER?
- Eliminates "not a git repository" errors
- Eliminates "No such file or directory" errors from wrong cwd
- Simpler syntax, fewer mistakes
- Works reliably every time

TROUBLESHOOTING - PERMISSION DENIED ERROR:
If you see "Permission denied" when running the wrapper script, fix it with:
```bash
chmod +x $HOME/${projname}_cmd.sh
```
Then retry the command. This has happened before and chmod resolves it.

**OPTION 2: MANUAL CD PREFIX (FALLBACK ONLY - USE ONLY IF WRAPPER FAILS)**

If the wrapper doesn't work, EVERY SINGLE Bash COMMAND MUST START WITH:
cd $HOME/${projname} &&

NO EXCEPTIONS. If you see an error like:
  "fatal: not a git repository (or any parent up to mount point /)"

STOP IMMEDIATELY. You forgot to cd to $HOME/${projname}.

Examples:
✓ CORRECT: cd $HOME/${projname} && git branch --show-current
✓ CORRECT: cd $HOME/${projname} && pwd && ls
✗ WRONG:   git branch --show-current
✗ WRONG:   pwd && git branch --show-current

⚠️⚠️⚠️ END MANDATORY RULE ⚠️⚠️⚠️

## THE PROBLEM

The Bash tool does NOT maintain working directory state between calls. Each Bash invocation starts fresh, and the system may reset cwd to different locations.

Observed behavior:
- Sometimes starts in: $HOME/${projname}
- Sometimes starts in: /work/gj26/b20090
- The cwd is unpredictable between Bash tool calls
- pwd output showing /work/gj26/b20090 means you are NOT in the git repo

## CRITICAL RULES FOR PATH HANDLING

### Rule 1: ALWAYS use absolute paths for file operations
✓ GOOD: bash $HOME/${projname}/tasks/rl/scripts/miyabi/miyabi_all.sh
✗ BAD:  bash tasks/rl/scripts/miyabi/miyabi_all.sh

### Rule 2: For scripts with relative path dependencies, cd WITHIN the same command
✓ GOOD: cd $HOME/${projname} && bash tasks/rl/scripts/miyabi/miyabi_all.sh
✗ BAD:  cd $HOME/${projname}
        bash tasks/rl/scripts/miyabi/miyabi_all.sh  # Different Bash call, cwd lost!

### Rule 3: Check pwd before making assumptions
✓ GOOD: pwd && ls tasks/rl/scripts/  # Verify location first
✗ BAD:  ls tasks/rl/scripts/          # Assumes unknown cwd

### Rule 4: Scripts that call other scripts need absolute paths or proper cwd
The miyabi_all.sh script contains:
  qsub -v ... tasks/rl/scripts/miyabi/train_single.sh

This REQUIRES running from $HOME/${projname} directory because:
- The path is relative
- qsub will look for the file relative to where the script was executed

SOLUTION: cd $HOME/${projname} && bash tasks/rl/scripts/miyabi/miyabi_all.sh

## WHY THIS HAPPENS

The Bash tool:
1. Does NOT persist working directory between invocations
2. Each call is a fresh shell session
3. System may set different default working directories
4. Cannot use 'cd' in one call and expect it to persist to the next

## CORRECT PATTERNS

### Pattern 1: Single command with cd
cd $HOME/${projname} && python -m tasks.rl.ppo_akorn ...

### Pattern 2: Absolute paths everywhere
python $HOME/${projname}/tasks/rl/ppo_akorn.py ...

### Pattern 3: Verify location first
pwd && cd $HOME/${projname} && bash script.sh

### Pattern 4: Chain dependent commands with &&
cd /path/to/dir && command1 && command2 && command3

## PROJECT-SPECIFIC PATHS

Base directory: $HOME/${projname}

Key locations:
- RL code:      $HOME/${projname}/tasks/rl/
- RL scripts:   $HOME/${projname}/tasks/rl/scripts/miyabi/
- Models:       $HOME/${projname}/models/
- Virtual env:  $HOME/${projname}/.venv_miyabi_two/

## LESSON LEARNED

When a script fails with "No such file or directory":
1. Check pwd first
2. Identify if paths in the script are relative or absolute
3. If relative, ensure you run from the correct directory using: cd /correct/path && script
4. If that fails, modify the script to use absolute paths

Never assume the working directory. Always verify or use absolute paths.

⚠️⚠️⚠️ PBS STDOUT HOUSEKEEPING ⚠️⚠️⚠️

1. At session start, if the directory `wandb_stdout/` does not exist in the working directory
   ($HOME/${projname}/), CREATE it:
   mkdir -p $HOME/${projname}/wandb_stdout

2. After ANY commands complete, check for PBS stdout files matching the pattern *.o[0-9]*
   (e.g., train_single.sh.o1176586, akorn_train.o1406897).
   The naming convention is <PBS_job_name>.o<job_id>.
   If found in the working directory, MOVE them to wandb_stdout/:
   mv $HOME/${projname}/*.o[0-9]* $HOME/${projname}/wandb_stdout/

⚠️⚠️⚠️ END PBS STDOUT HOUSEKEEPING ⚠️⚠️⚠️
