# The Starship Prompt & Watson Integration

## 1. System Overview
Starship is a lightning-fast, highly customizable terminal prompt written in Rust. In the Ultimate Stack, it replaces the default ugly bash prompt.
The configuration file is located at `~/.dotfiles/starship/.config/starship.toml` (and symlinked to `~/.config/starship.toml`).

## 2. The Custom Format
Your prompt is deliberately kept minimal to reduce cognitive load. It explicitly displays:
1. The current working directory.
2. Active Git branch & status.
3. Active Python environments or Node.js versions.
4. Command execution duration (if a command takes longer than 2 seconds).
5. **The Custom Watson Time-Tracker** (New addition).

## 3. Zero-Latency Watson Integration
Because Taskwarrior automatically starts a Watson timer in the background whenever you type `task 1 start`, it is very easy to forget that you are billing time. 

To solve this, we injected a custom module into the Starship prompt.
```toml
[custom.watson]
command = "grep -o '\"project\": \"[^\"]*\"' ~/.config/watson/state | cut -d'\"' -f4"
when = "test -s ~/.config/watson/state && grep -q 'project' ~/.config/watson/state"
format = "[⏱️ $output]($style) "
style = "bold yellow"
```

### Why it is built this way
If we simply told Starship to run `watson status -p` every time you press Enter, it would execute a Python script in the background. This would add ~150 milliseconds of lag to your terminal prompt, which breaks flow state.

Instead, we use a zero-latency bash hack. Watson saves its current state to a flat JSON file at `~/.config/watson/state`. 
The custom Starship module uses `grep` to read that flat file instantly. 
- **Latency**: 0ms. 
- **Result**: Whenever a timer is active, `[⏱️ Project_Name]` appears right above your typing cursor in bright yellow. When you stop the timer, it instantly disappears.
