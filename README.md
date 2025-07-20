Roblox-Asset-Preloader-Manager

🔥 Aggressive Asset Preloader for Roblox (with Sacrificial Clone Rendering)
This module, designed for use in ReplicatedStorage and triggered from StarterPlayerScripts, provides an aggressive asset preloading mechanism for Roblox games. It ensures that 3D assets like weapons and tools are fully loaded and rendered before gameplay starts, reducing in-game stutter and rendering delays.

🚀 Features:
Standard preloading via ContentProvider:PreloadAsync.

"Sacrificial Clone" technique: clones each asset temporarily, places it out of view, and parents it to the camera. This forces the rendering engine to cache the asset properly.

Runs over two passes to maximize asset readiness.
Skips empty folders safely and logs any failures.

📦 Assets Loaded:
All Model or Tool instances inside ReplicatedStorage.Weapons.

🛠 Usage:
Place this script in ReplicatedStorage (as a ModuleScript) and call Preloader.loadGameAssets() from a LocalScript inside StarterPlayerScripts.
The script must be modified to fit the folder reference, or position where the models are stored. If possible, inside ReplicatedStorage so that both server and client can access it.

NOTE: This asset preloader can be used for tools and veicles, when the player joins the game; to load ahead of time, the content.
In case of a 'Non fuctional' behavior, it can be edited and make complex anough to make it work, usually when loading assets for a cars dealership, or drop-tools systems.

System not working? 😡
1. Basic Checks
Script placement:

The ModuleScript is in ReplicatedStorage ?
The LocalScript calling Preloader.loadGameAssets() is in StarterPlayerScripts ?

Module usage:

local Preloader = require(game.ReplicatedStorage.Preloader)
Preloader.loadGameAssets()
Check for errors in F9 Developer Console (both client and server).

🧪 2. Debugging Suggestions
Problem: SetPrimaryPartCFrame errors?
Cause: The Model may not have a PrimaryPart set.

Solution: Add this check and fallback:

if not sacrificialClone.PrimaryPart then
    local primary = sacrificialClone:FindFirstChildWhichIsA("BasePart")
    if primary then
        sacrificialClone.PrimaryPart = primary
    else
        warn("Skipping asset, no PrimaryPart found:", asset.Name)
        continue
    end
end

❌ Problem: Nothing is cloned/rendered?
Cause: The camera might not exist yet, especially if the script runs too early.

Solution: Wait for CurrentCamera:

while not workspace.CurrentCamera do
    RunService.Heartbeat:Wait()
end

local camera = workspace.CurrentCamera

❌ Problem: Assets still lag when used?

Cause: PreloadAsync may not fully guarantee render-ready status.

Suggestions:
Increase delay after clone is parented:

RunService.Heartbeat:Wait()
task.wait(0.1) -- Slightly longer pause per asset

Try parenting to Workspace briefly instead of Camera, in case Camera parenting is ineffective on some devices:

sacrificialClone.Parent = workspace
🧼 3. Clean Alternative Approach (if this fails)
If rendering still stutters, consider:

StreamingEnabled settings: Preloading won't help if content is streamed in dynamically. Try setting StreamingEnabled = false in Workspace for full control.
Use of ViewportFrames: As a preview trick, load assets into a ViewportFrame off-screen to encourage rendering.

Preloading thumbnails: Sometimes rendering 2D previews triggers caching.
✅ Bonus: Testing Tip
Add visual confirmation for testing:

print("Loaded:", asset.Name)
Or display a loading screen while assets are processed.

PRO TIP: 🧠
Use this system to load game assets when player joins the server. This can help, roblox needs to render the object/model/mesh and also insert it inside of the workspace.
