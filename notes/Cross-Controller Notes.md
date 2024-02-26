What I know so far:

- Cross-Controller applications are locked to specific title IDs for the game they come with
  - This title ID is included in the application files (it does not appear to be part of LBP2)
    - You can find a list [here](https://github.com/vilijur/lbpcc-research/tree/main/TitleIDs)
- Updated copies of LBP2 include a CROSSDIR which contain the Cross Controller application as a pkg, with a signiture and icon. They are all called `DATA000`
- You can mix and match CROSSDIRS between LBP2 games (Including LBP HUB, which lacks a CROSSDIR)
- The Cross-Controller application requires a network check in order to function. It sends a request to a hardcoded URL (lbpvita.online.scee.com) to check internet connectivity, instead of using the system (Unknown reason why, possible leftover?)
  - The URL for checking internet connectivity is followed by sessionMaster (The state appears to be `true`, setting to false doesn't appear to change anything)
- LBP HUB has Cross-Controller functionality, however lacks a CROSSDIR. This causes an error when trying to install the application (C0-14347-9)
  - You can add a CROSSDIR from another LBP2 copy, however the application downloaded won't connect to LBP HUB due to a difference in Title IDs
  - If you were to change the Title ID of LBP HUB to that of the Title ID of the Cross-Controller application you have installed, you can start Cross-Controller on LBP HUB!
    - A game would have to be sacrified to being unplayable on the PS3 until you give HUB its orginal Title ID back. You may also need to rebuild your database for the modification to take effect.
      - Ideally, it would be possible to have a titleid.txt file with the title id of LBP HUB
- The Title ID in the application's file is encrypted for some reason (encryption method unknown, could be general vita encryption)

