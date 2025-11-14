# modules
The modules are sub-workflow processes that allow for linear top-level workflows.

## levels
The modules are broken into levels based on dependancy. Level 2 modules depend on level 1 modules, etc.

### level1
These are the modules you need to import first, they might depend on the errorManagement modules which you should have imported before the modules.
```
SWF003 - Generate Video Metadata.json
SWF007.1 - YT CHAN 1 Upload to YT.json
SWF007.2 - YT CHAN 2 Upload to YT.json
SWF008 - Download File for given URL.json
SWF013 - Add Slideshow Captions.json
SWF016.1 - Find GGL ACCT 1 GGL Drive Path.json
SWF016.2 - Find GGL ACCT 2 GGL Drive Path.json
SWF020 - Temporary Binary File Storage.json
SWF024 - Haves and Have Nots.json
SWF026.1 - Create GGL Sheets File GGL ACCT 1.json
SWF026.2 - Create GGL Sheets File GGL ACCT 2.json
SWF051 - Generate Long Story.json
SWF054 - LLM Completeness Check.json
SWF055 - Does File Exist.json
SWF056 - Create File.json
SWF057 - Get File Contents.json
```

### level2
These are the modules you ned to import second, they depend on the level1 modules.
```
SWF007 - Upload to YT.json
SWF010 - Generate Avatar.json
SWF014 - Generate Script.json
SWF016 - Find GGL Drive Path.json
SWF019 - Find GGL Document File.json
SWF023.1 - Update YT Playlist YT CHAN 1.json
SWF023.2 - Update YT Playlist YT CHAN 2.json
SWF026 - Create GGL Sheets File.json
SWF030 - Download And Save.json
SWF052 - Download And Save Temporarily.json
```

### level3
You guessed it, import these third, they depend on the level2 and level1 modules.
```
SWF009 - Upload to GGL.json
SWF011 - Generate Images.json
SWF012 - Generate Slideshow.json
SWF015 - Generate Thumbnail.json
SWF022 - Update Video Ledger Spreadsheet.json
SWF023 - Update YT Playlist.json
```

### level4
Import these fourth, dependant on level3, level2 and level1.
```
SWF017 - Update YT Video.json
SWF053 - Long Story Video Generation.json
```

### level5
Import these fifth, dependant on level4, level3, level2 and level1.
```
SWF021 - Upload Video.json
```

