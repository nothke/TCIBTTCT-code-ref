## I paid but I didn't get the game

A few customers have had issues with payment where they pay but never receive the game. That is a known issue with payment confirmation processing between Itch and PayPal. In that case, please contact me at nothke@gmail.com with the PayPal receipt (or just the buyer's PayPal account name is enough) so I can send you the key to activate the game. Note that I might be busy and not reply immediately, in which case I'm sorry in advance.

## Tower doesn't show up on start (fixed)

Update: This problem has been fixed as of version 9, and it had to do with some systems (depending where you're from) reading a decimal separator using ",".

Original text: Since the version 8, some users have had the problem where the tower doesn't show up on start. As the towers are now read from a text file, this is likely to be the problem with file reading permissions on your system.

If that happens, try to move the game into your documents folder, where your "user" is likely to have full reading and writing permissions. 

If that doesn't fix the issue, send a log file found at 
`C:\Users\username\AppData\LocalLow\Nothke\Tower\Player.log`
to me at nothke@gmail.com

## Loading MagicaVoxel files doesn't work

To load MagicaVoxel files, you MUST export them from MagicaVoxel as POINT .ply file, and save it into <GamePath>/MagicaVoxel/ folder

Be very careful because most example files have A LOT of cubes. You can see the cube count when you select the file and I don't suggest loading over 20k cubes.

## The resolution is tiny and the UI is now unreachable

When you change the resolution of the game, there is a 10 second timeout when you need to accept the resolution, otherwise the resolution will revert to the previously used one. That is a safety feature to prevent UI from being unreachable.
However, there could be a chance you lock the resolution to small one by either accepting accidentally or quitting the game while the countdown process is running.

The only way to change resolution now is to change it in the registry, where Unity stores the selected resolution.

1. Open registry editor (winkey + R to open the run window, type "regedit" hit enter).
2. now find HKEY_CURRENT_USER > Software > Nothke > Tower
3. There should be 2 keys Screenmanager Resolution Height_h2627697771 and Screenmanager Resolution Width_h182942802, those are your current height and width resolution
4. Open each and set to "decimal", now enter your desired screen resolutions and hit OK

## For other bugs - send me the log file

If you have any other bugs not mentioned above, please send a log file found at C:\Users\username\AppData\LocalLow\Nothke\Tower\Player.log to me at nothke@gmail.com
