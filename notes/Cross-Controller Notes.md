What I know so far:

- Cross-Controller applications are locked to specific title IDs for the game they come with
  - This title ID is included in the application files (it does not appear to be part of LBP2)
    - You can find a list [here](https://github.com/vilijur/lbpcc-research/tree/main/TitleIDs)
- Updated copies of LBP2 include a `CROSSDIR` directory which contain the Cross Controller application as a pkg, with a signiture and icon. They are all called `DATA000`
  - `CROSSDIR` is not apart of `USRDIR`
- You can mix and match `CROSSDIR`s between LBP2 games (Including LBP HUB, which lacks a `CROSSDIR`)
  - LBP3 lacks an option to start Cross-Controller, however since it's based off of LBP2, functionality for it may still exist unused
- The Cross-Controller application requires a network check in order to function. It sends a request to a hardcoded URL (`lbpvita.online.scee.com`) to check internet connectivity, instead of using the system (Unknown reason why, possible leftover?)
  - The URL for checking internet connectivity is followed by sessionMaster (The state appears to be `true`, setting to false doesn't appear to change anything)
  - This request is a DNS request. A DNS server can be set up to redirect to a different site that isn't dead to get it to work, like google.com.
    - Ideally, it would be possible to skip the network check entirely. Maybe a plugin can be used to modify memory?
- LBP HUB has Cross-Controller functionality, however lacks a `CROSSDIR`. This causes an error when trying to install the application (C0-14347-9)
  - You can add a `CROSSDIR` from another LBP2 copy, however the application downloaded won't connect to LBP HUB due to a difference in Title IDs
  - If you were to change the Title ID of LBP HUB to that of the Title ID of the Cross-Controller application you have installed, you can start Cross-Controller on LBP HUB!
    - A game would have to be sacrified to being unplayable on that PS3 until you give HUB its orginal Title ID back. You may also need to rebuild your database for the modification to take effect.
      - Ideally, it would be possible to have a `titleid.txt` file with the title id of LBP HUB
- The Title ID in the application's file is encrypted (General Vita Encryption)
  - The decrypted contents of `titleid.txt` is the Title ID of the version of LBP Vita it is based off of, and the Title ID for the LBP2 game to work with (This is the one the application comes with)
    - An example would be `PCSF00021BCES01086`
      - Anything longer than that will not be read (For example, you can't do something like `PCSF00021NPEA00437BCES01086` and have it work on both `NPEA00437` and `BCES01086`)
      - It doesn't appear to like when it's modified to use a different Title ID (won't enter the Cross-Controller menu on LBP2, causes loop)
      - However a new `titleid.txt` can be applied using rePatch, so that's handy (`rePatch/TITLEID/output/titleid.txt`)
    - These appear to be 18 bytes when decrypted
      - Doesn't seem like filesize matters?
  - ~~Not sure how to modify and re-encrypt this file...~~ re-encrypting is not required (yay rePatch)
  - It may be a good idea to remove this arbitary limitation at some point. Research would need to be done into how the application works to make a patch like this.
  - The Title ID can be found in `/output/titleid.txt`
    - Could it be possible that the game generates the Title ID? Food for thought.
 - Cross-Controller doesn't have it's own save data, instead relying on a file called `cross_controller_inventory.raw` to populate the player's inventory
   - Unsurprisingly, zeroing this file gives you nothing to work with in your inventory
   - I wonder if it's possible to modify this file to give all items in the game (assuming we don't already get everything)
