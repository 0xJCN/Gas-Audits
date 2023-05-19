## Summary
A majority of the optimizations were benchmarked via the protocol's tests. All benchmarked issues will include gas savings as seen from the output of `forge snapshot --diff`.

**Notes**: Some code snippets may be truncated to save space. 

Overall change to `canto-bio-protocol` from forge snapshot:
```
testMint() (gas: -23 (-0.020%))
testRevertOver200() (gas: -6 (-0.059%))
testRevertLen0() (gas: -6 (-0.061%))
testShortString(string) (gas: -1098 (-0.145%))
testEmojiAtBoundaries2() (gas: -1525 (-0.176%))
testLongString(string) (gas: -2052 (-0.207%))
testEmojiAtBoundaries() (gas: -2060 (-0.224%))
Overall gas change: -6770 (-0.184%)
```

Overall change to `canto-pfp-protocol` from forge snapshot:
```
testGetPFPWithNotMintedTokenID() (gas: 0 (0.000%))
testMintNFTOwnedByCaller() (gas: -31 (-0.011%))
testPFPNoLongerOwnedByOriginalOwner() (gas: -31 (-0.012%))
testNFTTransferredAfterwards() (gas: -31 (-0.012%))
testTokenURINFTOwnedByOwnerOfCIDNFT() (gas: -31 (-0.013%))
testPFPAssociatedWithNoCIDNFT() (gas: -31 (-0.016%))
testCannotMintNFTNotOwnedByCaller() (gas: -22151 (-24.664%))
Overall gas change: -22306 (-1.654%)
```

Overall change to `canto-namespace-protocol` from forge snapshot:
```
testChangeNoteAddressByNonOwner() (gas: 0 (0.000%))
testChangeNoteAddressByOwner() (gas: 0 (0.000%))
testChangeRevenueAddressByNonOwner() (gas: 0 (0.000%))
testChangeRevenueAddressByOwner() (gas: 0 (0.000%))
testFusingWith0Characters() (gas: 0 (0.000%))
testFusingWithMoreThan13Characters() (gas: 0 (0.000%))
testTokenURIOfNonMintedToken() (gas: 0 (0.000%))
testChangeNoteAddress() (gas: 0 (0.000%))
testChangeNoteAddressNonOwner() (gas: 0 (0.000%))
testChangeRevenueAddressByOwner() (gas: 0 (0.000%))
testEndPrelaunchPhaseByNonOwner() (gas: 0 (0.000%))
testRevertNonOwnerChangeRevenueAddress() (gas: 0 (0.000%))
testRevertUnminted() (gas: 0 (0.000%))
testRevertUnmintedGetTiles() (gas: 0 (0.000%))
testOwnerBuyTooManyInPrelaunchPhase() (gas: -9 (-0.047%))
testEndPrelaunchPhaseByOwner() (gas: -28 (-0.172%))
testTokenURIForNonPrelaunchTray() (gas: -3263 (-0.404%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -2397 (-0.466%))
testFusingAsApprovedOfTray() (gas: -2416 (-0.472%))
testBurnAsApprovedForAllOfOwner() (gas: -2541 (-0.503%))
testFusingWithOneTrayAndOneTile() (gas: -2417 (-0.509%))
testFusingAsOwnerOfTray() (gas: -2417 (-0.511%))
testBurnAsOwner() (gas: -2528 (-0.527%))
testFusingWithOneTrayAndMultiTiles() (gas: -2924 (-0.553%))
testTokenURIProperlyRendered() (gas: -2522 (-0.588%))
testTokenURIOfBurnedToken() (gas: -2453 (-0.618%))
testBurnNonPrelaunchTrayByApprovedForAll() (gas: -1955 (-0.619%))
testTokenURIForPrelaunchTrayDuringPrelaunch() (gas: -3215 (-0.621%))
testBurnNonPrelaunchTrayByApproved() (gas: -1960 (-0.624%))
testTransferNormalTray() (gas: -2198 (-0.630%))
testBurnNonPrelaunchTrayByOwner() (gas: -1953 (-0.666%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -2808 (-0.672%))
testBuyingOneAfterPrelaunchPhase() (gas: -2178 (-0.683%))
testBurnAsApproved() (gas: -4686 (-0.692%))
testTransferDuringPrelaunch() (gas: -2130 (-0.786%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -2589 (-0.802%))
testFusingWithDuplicateTiles() (gas: -2521 (-0.808%))
testFuseCharacterWithSkinToneModifier() (gas: -2543 (-0.812%))
testGetValidTiles() (gas: -2130 (-0.838%))
testFusingWithMuTrayAndMultiTiles() (gas: -9153 (-0.850%))
testGetTileBurned() (gas: -1879 (-0.862%))
testRevertTooHighTileOffset() (gas: -2139 (-0.868%))
testTransferAfterPrelaunch() (gas: -2191 (-0.869%))
testBurnInPrelaunchPhase() (gas: -1879 (-0.869%))
testBuyInPrelaunchPhase() (gas: -2139 (-0.876%))
testGetValidTile() (gas: -2130 (-0.880%))
testBurnPrelaunchTray() (gas: -1950 (-0.894%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -9998 (-0.915%))
testFusingDuringPrelaunch() (gas: -5601 (-0.965%))
testFuseWithEmojiSupportToneModifier() (gas: -11445 (-1.004%))
testBuyingMultipleOnesAfterPrelaunchPhase() (gas: -11558 (-1.164%))
testComplexScenario() (gas: -278460 (-1.319%))
testOwnerBuyMaxInPrelaunchPhase() (gas: -2344785 (-1.332%))
Overall gas change: -2744088 (-1.268%)
```

### Gas Optimizations
| Number |Issue|Instances|Estimated Gas Savings|
|-|:-|:-:|:-:|
| [G-01](#refactor-code-to-avoid-writing-to-and-reading-from-storage-in-for-loop) | Refactor code to avoid writing to and reading from storage in `for loop` | - | - |
| [G-02](#refactor-code-to-fail-early-and-save-5k-gas-for-users-that-input-wrong-parameters) | Refactor code to fail early and save `5k gas` for users that input wrong parameters | - | 5000 |
| [G-03](#cache-pointer-to-struct-in-calldatamemory-to-avoid-re-calculating-offset) | Cache pointer to struct in calldata/memory to avoid re-calculating offset | 6 | - |
| [G-04](#if-statements-that-use--can-be-refactored-into-nested-if-statements) | `If` statements that use `&&` can be refactored into nested `if` statements | 11 | - |
| [G-05](#expressions-can-be-unchecked-if-guranteed-to-not-overunderflow-due-to-previous-ifrequire-statement-or-other-constraints) | Expressions can be unchecked if guranteed to not over/underflow due to previous `if`/`require` statement or other constraints | 6 | - |
| [G-06](#cache-struct-value-from-return-data) | Cache struct value from return data | 1 | - |
| [G-07](#multiple-uintid-mappings-can-be-combined-into-a-single-mapping-of-a-uintid-to-a-struct-where-appropriate) | Multiple uint/ID mappings can be combined into a single mapping of a uint/ID to a struct, where appropriate | - | - |
| [G-08](#consider-creating-additional-burn-function-in-traysol-to-avoid-multiple-external-calls-in-for-loop) | Consider creating additional `burn` function in `Tray.sol` to avoid multiple external calls in for loop | - | - |
| [G-09](#consider-using-bytes32-to-set-variable-as-immutable-and-save-an-sload) | Consider using `bytes32` to set variable as immutable and save an SLOAD | 1 | 2100 |

## Refactor code to avoid writing to and reading from storage in `for loop`
Writing to storage and reading from storage should always try to be avoided in loops. In the following instance we are able to cache the value of `lastHash` outside of the loop to avoid 7 SSTOREs and 14 SLOADs on every iteration. We can then update `lastHash` by storing the cached value into storage once we are outisde of the loop.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L159-L166

*Gas Savings as shown via `forge snapshot --diff`*:
```
testOwnerBuyTooManyInPrelaunchPhase() (gas: -9 (-0.047%))
testBurnAsApproved() (gas: -1704 (-0.252%))
testTokenURIForNonPrelaunchTray() (gas: -2130 (-0.264%))
testFusingWithOneTrayAndMultiTiles() (gas: -1704 (-0.322%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -1704 (-0.331%))
testFusingAsApprovedOfTray() (gas: -1704 (-0.333%))
testBurnAsApprovedForAllOfOwner() (gas: -1704 (-0.337%))
testBurnAsOwner() (gas: -1704 (-0.355%))
testFusingWithOneTrayAndOneTile() (gas: -1704 (-0.359%))
testFusingAsOwnerOfTray() (gas: -1704 (-0.360%))
testFusingDuringPrelaunch() (gas: -2130 (-0.367%))
testTokenURIProperlyRendered() (gas: -1704 (-0.397%))
testTokenURIForPrelaunchTrayDuringPrelaunch() (gas: -2130 (-0.411%))
testTokenURIOfBurnedToken() (gas: -1704 (-0.430%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -2130 (-0.510%))
testBurnNonPrelaunchTrayByApprovedForAll() (gas: -1704 (-0.539%))
testBurnNonPrelaunchTrayByApproved() (gas: -1704 (-0.542%))
testBurnNonPrelaunchTrayByOwner() (gas: -1704 (-0.581%))
testTransferNormalTray() (gas: -2130 (-0.611%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -2130 (-0.660%))
testFusingWithMuTrayAndMultiTiles() (gas: -7160 (-0.665%))
testBuyingOneAfterPrelaunchPhase() (gas: -2130 (-0.668%))
testFuseCharacterWithSkinToneModifier() (gas: -2130 (-0.680%))
testFusingWithDuplicateTiles() (gas: -2130 (-0.683%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -8520 (-0.780%))
testBurnPrelaunchTray() (gas: -1704 (-0.781%))
testGetTileBurned() (gas: -1711 (-0.785%))
testTransferDuringPrelaunch() (gas: -2130 (-0.786%))
testBurnInPrelaunchPhase() (gas: -1711 (-0.792%))
testGetValidTiles() (gas: -2130 (-0.838%))
testTransferAfterPrelaunch() (gas: -2130 (-0.845%))
testRevertTooHighTileOffset() (gas: -2139 (-0.868%))
testBuyInPrelaunchPhase() (gas: -2139 (-0.876%))
testGetValidTile() (gas: -2130 (-0.880%))
testFuseWithEmojiSupportToneModifier() (gas: -10650 (-0.934%))
testBuyingMultipleOnesAfterPrelaunchPhase() (gas: -11510 (-1.159%))
testComplexScenario() (gas: -257520 (-1.219%))
testOwnerBuyMaxInPrelaunchPhase() (gas: -2344785 (-1.332%))
Overall gas change: -2699400 (-1.248%)
```
```solidity
File: canto-namespace-protocol/src/Tray.sol
159:        for (uint256 i; i < _amount; ++i) {
160:            TileData[TILES_PER_TRAY] memory trayTiledata;
161:            for (uint256 j; j < TILES_PER_TRAY; ++j) {
162:                lastHash = keccak256(abi.encode(lastHash));
163:                trayTiledata[j] = _drawing(uint256(lastHash));
164:            }
165:            tiles[startingTrayId + i] = trayTiledata;
166:        }
```
```diff
diff --git a/src/Tray.sol b/src/Tray.sol
index 4796cf6..1337443 100644
--- a/src/Tray.sol
+++ b/src/Tray.sol
@@ -156,14 +156,16 @@ contract Tray is ERC721A, Owned {
         } else {
             SafeTransferLib.safeTransferFrom(note, msg.sender, revenueAddress, _amount * trayPrice);
         }
+        bytes32 _lastHash = lastHash;
         for (uint256 i; i < _amount; ++i) {
             TileData[TILES_PER_TRAY] memory trayTiledata;
             for (uint256 j; j < TILES_PER_TRAY; ++j) {
-                lastHash = keccak256(abi.encode(lastHash));
-                trayTiledata[j] = _drawing(uint256(lastHash));
+                _lastHash = keccak256(abi.encode(_lastHash));
+                trayTiledata[j] = _drawing(uint256(_lastHash));
             }
             tiles[startingTrayId + i] = trayTiledata;
         }
+        lastHash = _lastHash;
         _mint(msg.sender, _amount); // We do not use _safeMint on purpose here to disallow callbacks and save gas
     }
```

## Refactor code to fail early and save `5k gas` for users that input wrong parameters
In the instance below an SLOAD and SSTORE is occuring before the access control check in the `if` statement. If the check causes a revert, the user who mistakenly called `mint` with invalid parameters will incur an additional gas cost of ~5000 gas (`Gcoldsload (2100 gas)` & `Gsreset (2900 gas)`). To save 1 SLOAD and 1 SSTORE for users who accidentally call `mint` with the wrong parameters, move the `if` statement to the very top of the function. 

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L80-L82

**Note that the ~22151 gas savings below are coming from the scenario where a user is the FIRST one to call mint AND uses invalid parameters (`Gsset (20000 gas)` & `Gcoldsload (2100 gas)`). After `numMinted` has been incremented once, every user who calls mint with invalid parameters will only incur an extra cost of ~5000 gas.**

*Gas Savings as shown via `forge snapshot --diff`*:
```
testGetPFPWithNotMintedTokenID() (gas: 0 (0.000%))
testMintNFTOwnedByCaller() (gas: -6 (-0.002%))
testPFPNoLongerOwnedByOriginalOwner() (gas: -6 (-0.002%))
testNFTTransferredAfterwards() (gas: -6 (-0.002%))
testTokenURINFTOwnedByOwnerOfCIDNFT() (gas: -6 (-0.002%))
testPFPAssociatedWithNoCIDNFT() (gas: -6 (-0.003%))
testCannotMintNFTNotOwnedByCaller() (gas: -22151 (-24.664%))
Overall gas change: -22181 (-1.645%)
```
```solidity
File: canto-pfp-protocol/src/ProfilePicture.sol
79:    function mint(address _nftContract, uint256 _nftID) external {
80:        uint256 tokenId = ++numMinted;
81:        if (ERC721(_nftContract).ownerOf(_nftID) != msg.sender)
82:            revert PFPNotOwnedByCaller(msg.sender, _nftContract, _nftID);
83:        ProfilePictureData storage pictureData = pfp[tokenId];
84:        pictureData.nftContract = _nftContract;
85:        pictureData.nftID = _nftID;
86:        _mint(msg.sender, tokenId);
87:        emit PfpAdded(msg.sender, tokenId, _nftContract, _nftID);
88:    }
```
```diff
diff --git a/src/ProfilePicture.sol b/src/ProfilePicture.sol
index 54d814a..e568879 100644
--- a/src/ProfilePicture.sol
+++ b/src/ProfilePicture.sol
@@ -77,9 +77,9 @@ contract ProfilePicture is ERC721 {
     /// @param _nftContract The nft contract address to reference
     /// @param _nftID The nft ID to reference
     function mint(address _nftContract, uint256 _nftID) external {
-        uint256 tokenId = ++numMinted;
         if (ERC721(_nftContract).ownerOf(_nftID) != msg.sender)
             revert PFPNotOwnedByCaller(msg.sender, _nftContract, _nftID);
+        uint256 tokenId = ++numMinted;
         ProfilePictureData storage pictureData = pfp[tokenId];
         pictureData.nftContract = _nftContract;
         pictureData.nftID = _nftID;
```

## Cache pointer to struct in calldata/memory to avoid re-calculating offset
Caching a mapping/array's value in a storage pointer when the value is accessed multiple times saves ~40 gas per access due to not having to perform the same offset calculation every time. In the instances below the offset calculation is happening multiple times within loops so the gas costs could vary depending on the number of iterations in the loop.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L122-L143

*Gas Savings as shown via `forge snapshot --diff`*:
```
testFuseWithEmojiSupportToneModifier() (gas: -196 (-0.017%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -159 (-0.031%))
testBurnAsApprovedForAllOfOwner() (gas: -157 (-0.031%))
testFusingAsApprovedOfTray() (gas: -160 (-0.031%))
testBurnAsOwner() (gas: -157 (-0.033%))
testFusingWithOneTrayAndOneTile() (gas: -157 (-0.033%))
testFusingAsOwnerOfTray() (gas: -157 (-0.033%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -376 (-0.034%))
testTokenURIProperlyRendered() (gas: -157 (-0.037%))
testTokenURIOfBurnedToken() (gas: -157 (-0.040%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -195 (-0.047%))
testFusingWithDuplicateTiles() (gas: -164 (-0.053%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -180 (-0.056%))
testFuseCharacterWithSkinToneModifier() (gas: -180 (-0.058%))
testFusingWithMuTrayAndMultiTiles() (gas: -827 (-0.077%))
testComplexScenario() (gas: -16512 (-0.078%))
testFusingWithOneTrayAndMultiTiles() (gas: -627 (-0.119%))
testBurnAsApproved() (gas: -2202 (-0.325%))
testFusingDuringPrelaunch() (gas: -2752 (-0.474%))
Overall gas change: -25472 (-0.012%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
122:        for (uint256 i; i < numCharacters; ++i) {
123:            bool isLastTrayEntry = true;
124:            uint256 trayID = _characterList[i].trayID; // @audit: 1st access
125:            uint8 tileOffset = _characterList[i].tileOffset; // @audit: 2nd access
126:            // Check for duplicate characters in the provided list. 1/2 * n^2 loop iterations, but n is bounded to 13 and we do not perform any storage operations
127:            for (uint256 j = i + 1; j < numCharacters; ++j) {
128:                if (_characterList[j].trayID == trayID) { // @audit: 1st access
129:                    isLastTrayEntry = false;
130:                    if (_characterList[j].tileOffset == tileOffset) revert FusingDuplicateCharactersNotAllowed(); // @audit: 2nd access
131:                }
132:            }
133:            Tray.TileData memory tileData = tray.getTile(trayID, tileOffset); // Will revert if tileOffset is too high
134:            uint8 characterModifier = tileData.characterModifier;
135:
136:            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) { // @audit: 3rd access
137:                revert CannotFuseCharacterWithSkinTone();
138:            }
139:            
140:            if (tileData.fontClass == 0) {
141:                // Emoji
142:                characterModifier = _characterList[i].skinToneModifier; // @audit: 4th access
143:            }
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..6a123c8 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -121,25 +121,27 @@ contract Namespace is ERC721, Owned {
         uint256[] memory uniqueTrays = new uint256[](_characterList.length);
         for (uint256 i; i < numCharacters; ++i) {
             bool isLastTrayEntry = true;
-            uint256 trayID = _characterList[i].trayID;
-            uint8 tileOffset = _characterList[i].tileOffset;
+            CharacterData calldata _characterData = _characterList[i];
+            uint256 trayID = _characterData.trayID;
+            uint8 tileOffset = _characterData.tileOffset;
             // Check for duplicate characters in the provided list. 1/2 * n^2 loop iterations, but n is bounded to 13 and we do not perform any storage operations
             for (uint256 j = i + 1; j < numCharacters; ++j) {
-                if (_characterList[j].trayID == trayID) {
+                CharacterData calldata _characterDataInner = _characterList[j];
+                if (_characterDataInner.trayID == trayID) {
                     isLastTrayEntry = false;
-                    if (_characterList[j].tileOffset == tileOffset) revert FusingDuplicateCharactersNotAllowed();
+                    if (_characterDataInner.tileOffset == tileOffset) revert FusingDuplicateCharactersNotAllowed();
                 }
             }
             Tray.TileData memory tileData = tray.getTile(trayID, tileOffset); // Will revert if tileOffset is too high
             uint8 characterModifier = tileData.characterModifier;

-            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) {
+            if (tileData.fontClass != 0 && _characterData.skinToneModifier != 0) {
                 revert CannotFuseCharacterWithSkinTone();
             }

             if (tileData.fontClass == 0) {
                 // Emoji
-                characterModifier = _characterList[i].skinToneModifier;
+                characterModifier = _characterData.skinToneModifier;
             }
             bytes memory charAsBytes = Utils.characterToUnicodeBytes(0, tileData.characterIndex, characterModifier);
             tileData.characterModifier = characterModifier;
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Utils.sol#L231-L233

*Gas Savings as shown via `forge snapshot --diff`*:
```
testSelectTrayIdToFuseSupportSkinTone() (gas: -149 (-0.014%))
testTokenURIProperlyRendered() (gas: -119 (-0.028%))
testTokenURIForNonPrelaunchTray() (gas: -1085 (-0.134%))
testTokenURIForPrelaunchTrayDuringPrelaunch() (gas: -1085 (-0.209%))
Overall gas change: -2438 (-0.001%)
```
```solidity
File: canto-namespace-protocol/src/Utils.sol
231:                string(
232:                    characterToUnicodeBytes(_tiles[i].fontClass, _tiles[i].characterIndex, _tiles[i].characterModifier) // @audit: 3 accesses
233:                ),
```
```diff
diff --git a/src/Utils.sol b/src/Utils.sol
index 66b61c6..d491b77 100644
--- a/src/Utils.sol
+++ b/src/Utils.sol
@@ -223,13 +223,14 @@ library Utils {
         string memory innerData;
         uint256 dx = 400 / (_tiles.length + 1);
         for (uint256 i; i < _tiles.length; ++i) {
+            Tray.TileData memory _tileData = _tiles[i];
             innerData = string.concat(
                 innerData,
                 '<text dominant-baseline="middle" text-anchor="middle" y="100" x="',
                 LibString.toString(dx * (i + 1)),
                 '">',
                 string(
-                    characterToUnicodeBytes(_tiles[i].fontClass, _tiles[i].characterIndex, _tiles[i].characterModifier)
+                    characterToUnicodeBytes(_tileData.fontClass, _tileData.characterIndex, _tileData.characterModifier)
                 ),
                 "</text>"
             );
```

## `If` statements that use `&&` can be refactored into nested `if` statements

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L175-L186

*Gas Savings as shown via `forge snapshot --diff`*:
```
testComplexScenario() (gas: -412 (-0.002%))
testFuseWithEmojiSupportToneModifier() (gas: -70 (-0.006%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -70 (-0.006%))
testBurnAsApproved() (gas: -50 (-0.007%))
testFusingWithOneTrayAndMultiTiles() (gas: -49 (-0.009%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -49 (-0.010%))
testBurnAsApprovedForAllOfOwner() (gas: -49 (-0.010%))
testFusingAsApprovedOfTray() (gas: -50 (-0.010%))
testBurnAsOwner() (gas: -50 (-0.010%))
testFusingWithOneTrayAndOneTile() (gas: -50 (-0.011%))
testFusingAsOwnerOfTray() (gas: -50 (-0.011%))
testBurnNonPrelaunchTrayByApprovedForAll() (gas: -36 (-0.011%))
testBurnNonPrelaunchTrayByOwner() (gas: -34 (-0.012%))
testFusingDuringPrelaunch() (gas: -70 (-0.012%))
testTokenURIProperlyRendered() (gas: -56 (-0.013%))
testBurnNonPrelaunchTrayByApproved() (gas: -41 (-0.013%))
testFusingWithMuTrayAndMultiTiles() (gas: -149 (-0.014%))
testTokenURIOfBurnedToken() (gas: -56 (-0.014%))
testBurnPrelaunchTray() (gas: -34 (-0.016%))
testGetTileBurned() (gas: -34 (-0.016%))
testBurnInPrelaunchPhase() (gas: -35 (-0.016%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -89 (-0.021%))
Overall gas change: -1583 (-0.001%)
```
```solidity
File: canto-namespace-protocol/src/Tray.sol
175:        if (
176:            namespaceNFT != msg.sender &&
177:            trayOwner != msg.sender &&
178:            getApproved(_id) != msg.sender &&
179:            !isApprovedForAll(trayOwner, msg.sender)
180:        ) revert CallerNotAllowedToBurn();
181:        if (msg.sender == namespaceNFT) {
182:            // Disallow fusing for prelaunch trays after phase has ended
183:            uint256 numPrelaunchMinted = prelaunchMinted;
184:            if (numPrelaunchMinted != type(uint256).max && _id <= numPrelaunchMinted)
185:                revert PrelaunchTrayCannotBeUsedAfterPrelaunch(_id);
186:        }
```
```diff
diff --git a/src/Tray.sol b/src/Tray.sol
index 4796cf6..553c0ed 100644
--- a/src/Tray.sol
+++ b/src/Tray.sol
@@ -172,17 +172,23 @@ contract Tray is ERC721A, Owned {
     /// @param _id Tray ID
     function burn(uint256 _id) external {
         address trayOwner = ownerOf(_id);
-        if (
-            namespaceNFT != msg.sender &&
-            trayOwner != msg.sender &&
-            getApproved(_id) != msg.sender &&
-            !isApprovedForAll(trayOwner, msg.sender)
-        ) revert CallerNotAllowedToBurn();
+        if (namespaceNFT != msg.sender) {
+            if (trayOwner != msg.sender) {
+                if (getApproved(_id) != msg.sender) {
+                    if (!isApprovedForAll(trayOwner, msg.sender)) {
+                        revert CallerNotAllowedToBurn();
+                    }
+                }
+            }
+        }
         if (msg.sender == namespaceNFT) {
             // Disallow fusing for prelaunch trays after phase has ended
             uint256 numPrelaunchMinted = prelaunchMinted;
-            if (numPrelaunchMinted != type(uint256).max && _id <= numPrelaunchMinted)
-                revert PrelaunchTrayCannotBeUsedAfterPrelaunch(_id);
+            if (numPrelaunchMinted != type(uint256).max) {
+                if (_id <= numPrelaunchMinted) {
+                    revert PrelaunchTrayCannotBeUsedAfterPrelaunch(_id);
+                }
+            }
         }
         delete tiles[_id];
         _burn(_id);
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L136-L138

*Gas Savings as shown via `forge snapshot --diff`*:
```
testFuseWithEmojiSupportToneModifier() (gas: -22 (-0.002%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -20 (-0.004%))
testFusingAsApprovedOfTray() (gas: -20 (-0.004%))
testBurnAsApprovedForAllOfOwner() (gas: -20 (-0.004%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -44 (-0.004%))
testBurnAsOwner() (gas: -20 (-0.004%))
testFusingWithOneTrayAndOneTile() (gas: -20 (-0.004%))
testFusingAsOwnerOfTray() (gas: -20 (-0.004%))
testTokenURIProperlyRendered() (gas: -20 (-0.005%))
testComplexScenario() (gas: -1035 (-0.005%))
testTokenURIOfBurnedToken() (gas: -20 (-0.005%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -25 (-0.006%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -22 (-0.007%))
testFuseCharacterWithSkinToneModifier() (gas: -27 (-0.009%))
testFusingWithMuTrayAndMultiTiles() (gas: -95 (-0.009%))
testFusingWithOneTrayAndMultiTiles() (gas: -55 (-0.010%))
testBurnAsApproved() (gas: -136 (-0.020%))
testFusingDuringPrelaunch() (gas: -169 (-0.029%))
Overall gas change: -1790 (-0.001%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
136:            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) {
137:                revert CannotFuseCharacterWithSkinTone();
138:            }
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..1a66ca7 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -133,8 +133,10 @@ contract Namespace is ERC721, Owned {
             Tray.TileData memory tileData = tray.getTile(trayID, tileOffset); // Will revert if tileOffset is too high
             uint8 characterModifier = tileData.characterModifier;

-            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) {
-                revert CannotFuseCharacterWithSkinTone();
+            if (tileData.fontClass != 0) {
+                if (_characterList[i].skinToneModifier !=0) {
+                    revert CannotFuseCharacterWithSkinTone();
+                }
             }
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L157-L161

*Gas Savings as shown via `forge snapshot --diff`*:
```
testComplexScenario() (gas: -204 (-0.001%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -5 (-0.001%))
testFuseWithEmojiSupportToneModifier() (gas: -34 (-0.003%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -34 (-0.003%))
testBurnAsApproved() (gas: -28 (-0.004%))
testFusingAsApprovedOfTray() (gas: -24 (-0.005%))
testFusingWithOneTrayAndMultiTiles() (gas: -27 (-0.005%))
testBurnAsApprovedForAllOfOwner() (gas: -27 (-0.005%))
testBurnAsOwner() (gas: -27 (-0.006%))
testFusingWithOneTrayAndOneTile() (gas: -27 (-0.006%))
testFusingDuringPrelaunch() (gas: -34 (-0.006%))
testFusingAsOwnerOfTray() (gas: -28 (-0.006%))
testTokenURIProperlyRendered() (gas: -27 (-0.006%))
testTokenURIOfBurnedToken() (gas: -27 (-0.007%))
testFusingWithMuTrayAndMultiTiles() (gas: -82 (-0.008%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -34 (-0.008%))
Overall gas change: -669 (-0.000%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
157:                if (
158:                    trayOwner != msg.sender &&
159:                    tray.getApproved(trayID) != msg.sender &&
160:                    !tray.isApprovedForAll(trayOwner, msg.sender)
161:                ) revert CallerNotAllowedToFuse();
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..0ffe348 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -154,11 +154,13 @@ contract Namespace is ERC721, Owned {
                 uniqueTrays[numUniqueTrays++] = trayID;
                 // Verify address is allowed to fuse
                 address trayOwner = tray.ownerOf(trayID);
-                if (
-                    trayOwner != msg.sender &&
-                    tray.getApproved(trayID) != msg.sender &&
-                    !tray.isApprovedForAll(trayOwner, msg.sender)
-                ) revert CallerNotAllowedToFuse();
+                if (trayOwner != msg.sender) {
+                    if (tray.getApproved(trayID) != msg.sender) {
+                        if (!tray.isApprovedForAll(trayOwner, msg.sender)) {
+                            revert CallerNotAllowedToFuse();
+                        }
+                    }
+                }
             }
         }
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L186-L187

*Gas Savings as shown via `forge snapshot --diff`*:
```
testTransferNormalTray() (gas: 0 (0.000%))
testBurnAsApproved() (gas: -40 (-0.006%))
testBurnAsApprovedForAllOfOwner() (gas: -34 (-0.007%))
testBurnAsOwner() (gas: -33 (-0.007%))
testTokenURIOfBurnedToken() (gas: -33 (-0.008%))
Overall gas change: -140 (-0.000%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
186:        if (nftOwner != msg.sender && getApproved[_id] != msg.sender && !isApprovedForAll[nftOwner][msg.sender])
187:            revert CallerNotAllowedToBurn();
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..381e0a9 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -183,8 +183,15 @@ contract Namespace is ERC721, Owned {
     /// @param _id Namespace NFT ID
     function burn(uint256 _id) external {
         address nftOwner = ownerOf(_id);
-        if (nftOwner != msg.sender && getApproved[_id] != msg.sender && !isApprovedForAll[nftOwner][msg.sender])
-            revert CallerNotAllowedToBurn();
+        if (nftOwner != msg.sender) {
+            if (getApproved[_id] != msg.sender) {
+                if (!isApprovedForAll[nftOwner][msg.sender]) {
+                    revert CallerNotAllowedToBurn();
+                }
+            }
+        }
         string memory associatedName = tokenToName[_id];
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L60-L95

*Gas Savings as shown via `forge snapshot --diff`*:
```
testShortString(string) (gas: -785 (-0.103%))
testEmojiAtBoundaries2() (gas: -1092 (-0.126%))
testLongString(string) (gas: -1546 (-0.156%))
testEmojiAtBoundaries() (gas: -1449 (-0.157%))
Overall gas change: -4872 (-0.133%)
```
```solidity
File: canto-bio-protocol/src/Bio.sol
60:            if ((i > 0 && (i + 1) % 40 == 0) || prevByteWasContinuation || i == lengthInBytes - 1) {
61:                bytes1 nextCharacter;
62:                if (i != lengthInBytes - 1) {
63:                    nextCharacter = bioTextBytes[i + 1];
64:                }
65:                if (nextCharacter & 0xC0 == 0x80) {
66:                    // Unicode continuation byte, top two bits are 10
67:                    prevByteWasContinuation = true;
68:                } else {
69:                    // Do not split when the prev. or next character is a zero width joiner. Otherwise, 👨‍👧‍👦 could become 👨>‍👧‍👦
70:                    // Furthermore, do not split when next character is skin tone modifier to avoid 🤦‍♂️\n🏻
71:                    if (
72:                        // Note that we do not need to check i < lengthInBytes - 4, because we assume that it's a valid UTF8 string and these prefixes imply that another byte follows
73:                        (nextCharacter == 0xE2 && bioTextBytes[i + 2] == 0x80 && bioTextBytes[i + 3] == 0x8D) ||
74:                        (nextCharacter == 0xF0 &&
75:                            bioTextBytes[i + 2] == 0x9F &&
76:                            bioTextBytes[i + 3] == 0x8F &&
77:                            uint8(bioTextBytes[i + 4]) >= 187 &&
78:                            uint8(bioTextBytes[i + 4]) <= 191) ||
79:                        (i >= 2 &&
80:                            bioTextBytes[i - 2] == 0xE2 &&
81:                            bioTextBytes[i - 1] == 0x80 &&
82:                            bioTextBytes[i] == 0x8D)
83:                    ) {
84:                        prevByteWasContinuation = true;
85:                        continue;
86:                    }
87:                    assembly {
88:                        mstore(bytesLines, bytesOffset)
89:                    }
90:                    strLines[insertedLines++] = string(bytesLines);
91:                    bytesLines = new bytes(80);
92:                    prevByteWasContinuation = false;
93:                    bytesOffset = 0;
94:                }
95:            }
```
```diff
diff --git a/src/Bio.sol b/src/Bio.sol
index 6d5757e..eb34f79 100644
--- a/src/Bio.sol
+++ b/src/Bio.sol
@@ -57,40 +57,42 @@ contract Bio is ERC721 {
             bytes1 character = bioTextBytes[i];
             bytesLines[bytesOffset] = character;
             bytesOffset++;
-            if ((i > 0 && (i + 1) % 40 == 0) || prevByteWasContinuation || i == lengthInBytes - 1) {
-                bytes1 nextCharacter;
-                if (i != lengthInBytes - 1) {
-                    nextCharacter = bioTextBytes[i + 1];
-                }
-                if (nextCharacter & 0xC0 == 0x80) {
-                    // Unicode continuation byte, top two bits are 10
-                    prevByteWasContinuation = true;
-                } else {
-                    // Do not split when the prev. or next character is a zero width joiner. Otherwise, 👨‍👧‍👦 could become 👨>‍👧‍👦
-                    // Furthermore, do not split when next character is skin tone modifier to avoid 🤦‍♂️\n🏻
-                    if (
-                        // Note that we do not need to check i < lengthInBytes - 4, because we assume that it's a valid UTF8 string and these prefixes imply that another byte follows
-                        (nextCharacter == 0xE2 && bioTextBytes[i + 2] == 0x80 && bioTextBytes[i + 3] == 0x8D) ||
-                        (nextCharacter == 0xF0 &&
-                            bioTextBytes[i + 2] == 0x9F &&
-                            bioTextBytes[i + 3] == 0x8F &&
-                            uint8(bioTextBytes[i + 4]) >= 187 &&
-                            uint8(bioTextBytes[i + 4]) <= 191) ||
-                        (i >= 2 &&
-                            bioTextBytes[i - 2] == 0xE2 &&
-                            bioTextBytes[i - 1] == 0x80 &&
-                            bioTextBytes[i] == 0x8D)
-                    ) {
-                        prevByteWasContinuation = true;
-                        continue;
+            if (i > 0) {
+                if ((i + 1) % 40 == 0 || prevByteWasContinuation || i == lengthInBytes - 1) {
+                    bytes1 nextCharacter;
+                    if (i != lengthInBytes - 1) {
+                        nextCharacter = bioTextBytes[i + 1];
                     }
-                    assembly {
-                        mstore(bytesLines, bytesOffset)
+                    if (nextCharacter & 0xC0 == 0x80) {
+                        // Unicode continuation byte, top two bits are 10
+                        prevByteWasContinuation = true;
+                    } else {
+                        // Do not split when the prev. or next character is a zero width joiner. Otherwise, 👨‍👧‍👦 could become 👨>‍👧‍👦
+                        // Furthermore, do not split when next character is skin tone modifier to avoid 🤦‍♂️\n🏻
+                        if (
+                            // Note that we do not need to check i < lengthInBytes - 4, because we assume that it's a valid UTF8 string and these prefixes imply that another byt
e follows
+                            (nextCharacter == 0xE2 && bioTextBytes[i + 2] == 0x80 && bioTextBytes[i + 3] == 0x8D) ||
+                            (nextCharacter == 0xF0 &&
+                                bioTextBytes[i + 2] == 0x9F &&
+                                bioTextBytes[i + 3] == 0x8F &&
+                                uint8(bioTextBytes[i + 4]) >= 187 &&
+                                uint8(bioTextBytes[i + 4]) <= 191) ||
+                            (i >= 2 &&
+                                bioTextBytes[i - 2] == 0xE2 &&
+                                bioTextBytes[i - 1] == 0x80 &&
+                                bioTextBytes[i] == 0x8D)
+                        ) {
+                            prevByteWasContinuation = true;
+                            continue;
+                        }
+                        assembly {
+                            mstore(bytesLines, bytesOffset)
+                        }
+                        strLines[insertedLines++] = string(bytesLines);
+                        bytesLines = new bytes(80);
+                        prevByteWasContinuation = false;
+                        bytesOffset = 0;
                     }
-                    strLines[insertedLines++] = string(bytesLines);
-                    bytesLines = new bytes(80);
-                    prevByteWasContinuation = false;
-                    bytesOffset = 0;
                 }
             }
         }
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L238-L242

*Gas Savings as shown via `forge snapshot --diff`*:
```
testBuyingMultipleOnesAfterPrelaunchPhase() (gas: -20 (-0.002%))
testTokenURIForNonPrelaunchTray() (gas: -20 (-0.002%))
testComplexScenario() (gas: -1075 (-0.005%))
testBuyingOneAfterPrelaunchPhase() (gas: -20 (-0.006%))
testTransferNormalTray() (gas: -40 (-0.011%))
testTransferAfterPrelaunch() (gas: -33 (-0.013%))
testFuseWithEmojiSupportToneModifier() (gas: -167 (-0.015%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -167 (-0.015%))
testBurnAsApproved() (gas: -192 (-0.028%))
testFusingDuringPrelaunch() (gas: -167 (-0.029%))
testTokenURIProperlyRendered() (gas: -134 (-0.031%))
testTokenURIOfBurnedToken() (gas: -133 (-0.034%))
testFusingWithOneTrayAndMultiTiles() (gas: -192 (-0.036%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -192 (-0.037%))
testFusingAsApprovedOfTray() (gas: -192 (-0.037%))
testBurnAsApprovedForAllOfOwner() (gas: -192 (-0.038%))
testBurnAsOwner() (gas: -192 (-0.040%))
testFusingWithOneTrayAndOneTile() (gas: -192 (-0.040%))
testFusingAsOwnerOfTray() (gas: -192 (-0.041%))
testFusingWithMuTrayAndMultiTiles() (gas: -560 (-0.052%))
testBurnNonPrelaunchTrayByApprovedForAll() (gas: -192 (-0.061%))
testBurnNonPrelaunchTrayByApproved() (gas: -192 (-0.061%))
testGetTileBurned() (gas: -134 (-0.061%))
testBurnInPrelaunchPhase() (gas: -134 (-0.062%))
testBurnNonPrelaunchTrayByOwner() (gas: -192 (-0.065%))
testBurnPrelaunchTray() (gas: -189 (-0.087%))
Overall gas change: -5105 (-0.002%)
```
```solidity
File: canto-namespace-protocol/src/Tray.sol
238:        if (numPrelaunchMinted != type(uint256).max) {
239:            // We do not allow any transfers of the prelaunch trays after the phase has ended
240:            if (startTokenId <= numPrelaunchMinted && to != address(0))
241:                revert PrelaunchTrayCannotBeUsedAfterPrelaunch(startTokenId);
242:        }
```
```diff
diff --git a/src/Tray.sol b/src/Tray.sol
index 4796cf6..6c554df 100644
--- a/src/Tray.sol
+++ b/src/Tray.sol
@@ -237,8 +237,11 @@ contract Tray is ERC721A, Owned {
         uint256 numPrelaunchMinted = prelaunchMinted;
         if (numPrelaunchMinted != type(uint256).max) {
             // We do not allow any transfers of the prelaunch trays after the phase has ended
-            if (startTokenId <= numPrelaunchMinted && to != address(0))
-                revert PrelaunchTrayCannotBeUsedAfterPrelaunch(startTokenId);
+            if (startTokenId <= numPrelaunchMinted) {
+                if (to != address(0)) {
+                    revert PrelaunchTrayCannotBeUsedAfterPrelaunch(startTokenId);
+                }
+            }
         }
     }
```

## Expressions can be unchecked if guranteed to not over/underflow due to previous `if`/`require` statement or other constraints

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L112-L113

*Gas Savings as shown via `forge snapshot --diff`*:
```
testComplexScenario() (gas: -1032 (-0.005%))
testFusingWithMuTrayAndMultiTiles() (gas: -138 (-0.013%))
testFuseWithEmojiSupportToneModifier() (gas: -172 (-0.015%))
testBurnAsApproved() (gas: -138 (-0.020%))
testFusingWithOneTrayAndMultiTiles() (gas: -137 (-0.026%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -137 (-0.027%))
testFusingAsApprovedOfTray() (gas: -138 (-0.027%))
testBurnAsApprovedForAllOfOwner() (gas: -137 (-0.027%))
testBurnAsOwner() (gas: -138 (-0.029%))
testFusingWithOneTrayAndOneTile() (gas: -138 (-0.029%))
testFusingAsOwnerOfTray() (gas: -138 (-0.029%))
testFusingDuringPrelaunch() (gas: -172 (-0.030%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -344 (-0.031%))
testTokenURIProperlyRendered() (gas: -138 (-0.032%))
testTokenURIOfBurnedToken() (gas: -137 (-0.035%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -172 (-0.041%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -172 (-0.053%))
testFuseCharacterWithSkinToneModifier() (gas: -172 (-0.055%))
testFusingWithDuplicateTiles() (gas: -172 (-0.055%))
Overall gas change: -3922 (-0.002%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
112:        if (numCharacters > 13 || numCharacters == 0) revert InvalidNumberOfCharacters(numCharacters);
113:        uint256 fusingCosts = 2**(13 - numCharacters) * 1e18;
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..bec1d13 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -110,7 +110,10 @@ contract Namespace is ERC721, Owned {
     function fuse(CharacterData[] calldata _characterList) external {
         uint256 numCharacters = _characterList.length;
         if (numCharacters > 13 || numCharacters == 0) revert InvalidNumberOfCharacters(numCharacters);
-        uint256 fusingCosts = 2**(13 - numCharacters) * 1e18;
+        uint256 fusingCosts;
+        unchecked {
+            fusingCosts = 2**(13 - numCharacters) * 1e18;
+        }
         SafeTransferLib.safeTransferFrom(note, msg.sender, revenueAddress, fusingCosts);
         uint256 namespaceIDToMint = ++nextNamespaceIDToMint;
         Tray.TileData[] storage nftToMintCharacters = nftCharacters[namespaceIDToMint];
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L115

*Gas Savings as shown via `forge snapshot --diff`*:
```
testComplexScenario() (gas: -312 (-0.001%))
testFusingWithMuTrayAndMultiTiles() (gas: -42 (-0.004%))
testFuseWithEmojiSupportToneModifier() (gas: -52 (-0.005%))
testBurnAsApproved() (gas: -42 (-0.006%))
testFusingWithOneTrayAndMultiTiles() (gas: -41 (-0.008%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -41 (-0.008%))
testBurnAsApprovedForAllOfOwner() (gas: -41 (-0.008%))
testFusingAsApprovedOfTray() (gas: -42 (-0.008%))
testBurnAsOwner() (gas: -42 (-0.009%))
testFusingWithOneTrayAndOneTile() (gas: -42 (-0.009%))
testFusingAsOwnerOfTray() (gas: -42 (-0.009%))
testFusingDuringPrelaunch() (gas: -52 (-0.009%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -104 (-0.010%))
testTokenURIProperlyRendered() (gas: -42 (-0.010%))
testTokenURIOfBurnedToken() (gas: -41 (-0.010%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -52 (-0.012%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -52 (-0.016%))
testFuseCharacterWithSkinToneModifier() (gas: -52 (-0.017%))
testFusingWithDuplicateTiles() (gas: -52 (-0.017%))
Overall gas change: -1186 (-0.001%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
115:        uint256 namespaceIDToMint = ++nextNamespaceIDToMint;
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..02d5731 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -112,7 +112,10 @@ contract Namespace is ERC721, Owned {
         if (numCharacters > 13 || numCharacters == 0) revert InvalidNumberOfCharacters(numCharacters);
         uint256 fusingCosts = 2**(13 - numCharacters) * 1e18;
         SafeTransferLib.safeTransferFrom(note, msg.sender, revenueAddress, fusingCosts);
-        uint256 namespaceIDToMint = ++nextNamespaceIDToMint;
+        uint256 namespaceIDToMint;
+        unchecked {
+            namespaceIDToMint = ++nextNamespaceIDToMint;
+        }
         Tray.TileData[] storage nftToMintCharacters = nftCharacters[namespaceIDToMint];
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L80

*Gas Savings as shown via `forge snapshot --diff`*:
```
testMintNFTOwnedByCaller() (gas: -25 (-0.009%))
testPFPNoLongerOwnedByOriginalOwner() (gas: -25 (-0.010%))
testNFTTransferredAfterwards() (gas: -25 (-0.010%))
testTokenURINFTOwnedByOwnerOfCIDNFT() (gas: -25 (-0.010%))
testPFPAssociatedWithNoCIDNFT() (gas: -25 (-0.013%))
testCannotMintNFTNotOwnedByCaller() (gas: -25 (-0.028%))
Overall gas change: -150 (-0.011%)
```
```solidity
File: canto-pfp-protocol/src/ProfilePicture.sol
80:        uint256 tokenId = ++numMinted;
```
```diff
diff --git a/src/ProfilePicture.sol b/src/ProfilePicture.sol
index 54d814a..0df694d 100644
--- a/src/ProfilePicture.sol
+++ b/src/ProfilePicture.sol
@@ -77,7 +77,10 @@ contract ProfilePicture is ERC721 {
     /// @param _nftContract The nft contract address to reference
     /// @param _nftID The nft ID to reference
     function mint(address _nftContract, uint256 _nftID) external {
-        uint256 tokenId = ++numMinted;
+        uint256 tokenId;
+        unchecked {
+            tokenId = ++numMinted;
+        }
         if (ERC721(_nftContract).ownerOf(_nftID) != msg.sender)
             revert PFPNotOwnedByCaller(msg.sender, _nftContract, _nftID);
         ProfilePictureData storage pictureData = pfp[tokenId];
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L124

*Gas Savings as shown via `forge snapshot --diff`*:
```
testLongString(string) (gas: -46 (-0.005%))
testEmojiAtBoundaries2() (gas: -43 (-0.005%))
testEmojiAtBoundaries() (gas: -46 (-0.005%))
testShortString(string) (gas: -43 (-0.006%))
testMint() (gas: -23 (-0.020%))
testRevertOver200() (gas: -6 (-0.059%))
testRevertLen0() (gas: -6 (-0.061%))
Overall gas change: -213 (-0.006%)
```
```solidity
File: canto-bio-protocol/src/Bio.sol
80:        uint256 tokenId = ++numMinted;
```
```diff
diff --git a/src/Bio.sol b/src/Bio.sol
index 6d5757e..f1e7caa 100644
--- a/src/Bio.sol
+++ b/src/Bio.sol
@@ -121,9 +121,13 @@ contract Bio is ERC721 {
     function mint(string calldata _bio) external {
         // We check the length in bytes, so will be higher for UTF-8 characters. But sufficient for this check
         if (bytes(_bio).length == 0 || bytes(_bio).length > 200) revert InvalidBioLength(bytes(_bio).length);
-        uint256 tokenId = ++numMinted;
+        uint256 tokenId;
+        unchecked {
+            tokenId = ++numMinted;
+        }
         bio[tokenId] = _bio;
         _mint(msg.sender, tokenId);
         emit BioAdded(msg.sender, tokenId, _bio);
     }
 }
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L49

*Gas Savings as shown via `forge snapshot --diff`*:
```
testShortString(string) (gas: -270 (-0.036%))
testEmojiAtBoundaries2() (gas: -390 (-0.045%))
testLongString(string) (gas: -460 (-0.046%))
testEmojiAtBoundaries() (gas: -565 (-0.061%))
Overall gas change: -1685 (-0.046%)
```
```solidity
File: canto-bio-protocol/src/Bio.sol
49:        uint lines = (lengthInBytes - 1) / 40 + 1;
```
```diff
diff --git a/src/Bio.sol b/src/Bio.sol
index 6d5757e..23f56f1 100644
--- a/src/Bio.sol
+++ b/src/Bio.sol
@@ -46,7 +46,10 @@ contract Bio is ERC721 {
         bytes memory bioTextBytes = bytes(bioText);
         uint lengthInBytes = bioTextBytes.length;
         // Insert a new line after 40 characters, taking into account unicode character
-        uint lines = (lengthInBytes - 1) / 40 + 1;
+        uint lines;
+        unchecked {
+            lines = (lengthInBytes - 1) / 40 + 1;
+        }
         string[] memory strLines = new string[](lines);
         bool prevByteWasContinuation;
         uint256 insertedLines;
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L227

**Note that this instance can not underflow since `_nextTokenId()` is defined to start at 1 and is only incremented**

*Gas Savings as shown via `forge snapshot --diff`*:
```
testComplexScenario() (gas: -28 (-0.000%))
testFusingWithMuTrayAndMultiTiles() (gas: -23 (-0.002%))
testBuyingMultipleOnesAfterPrelaunchPhase() (gas: -28 (-0.003%))
testBurnAsApproved() (gas: -23 (-0.003%))
testTokenURIForNonPrelaunchTray() (gas: -28 (-0.003%))
testFusingWithOneTrayAndMultiTiles() (gas: -22 (-0.004%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -22 (-0.004%))
testBurnAsApprovedForAllOfOwner() (gas: -22 (-0.004%))
testFusingAsApprovedOfTray() (gas: -23 (-0.004%))
testFusingWithOneTrayAndOneTile() (gas: -22 (-0.005%))
testBurnAsOwner() (gas: -23 (-0.005%))
testFusingAsOwnerOfTray() (gas: -23 (-0.005%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -28 (-0.007%))
testBurnNonPrelaunchTrayByApproved() (gas: -22 (-0.007%))
testBurnNonPrelaunchTrayByApprovedForAll() (gas: -23 (-0.007%))
testBurnNonPrelaunchTrayByOwner() (gas: -22 (-0.007%))
testTransferNormalTray() (gas: -28 (-0.008%))
testBuyingOneAfterPrelaunchPhase() (gas: -28 (-0.009%))
testBurnPrelaunchTray() (gas: -22 (-0.010%))
testTransferAfterPrelaunch() (gas: -28 (-0.011%))
testEndPrelaunchPhaseByOwner() (gas: -28 (-0.172%))
Overall gas change: -516 (-0.000%)
```
```solidity
File: canto-namespace-protocol/src/Tray.sol
227:      prelaunchMinted = _nextTokenId() - 1;
```
```diff
diff --git a/src/Tray.sol b/src/Tray.sol
index 4796cf6..02db6da 100644
--- a/src/Tray.sol
+++ b/src/Tray.sol
@@ -224,7 +224,9 @@ contract Tray is ERC721A, Owned {
     /// @notice End the prelaunch phase and start the public mint
     function endPrelaunchPhase() external onlyOwner {
         if (prelaunchMinted != type(uint256).max) revert PrelaunchAlreadyEnded();
-        prelaunchMinted = _nextTokenId() - 1;
+        unchecked {
+            prelaunchMinted = _nextTokenId() - 1;
+        }
         emit PrelaunchEnded();
     }
```

## Cache struct value from return data
In the instance below we can avoid re-reading struct values from the return value that was stored into memory by caching the values and using those instead. This will save us from doing offset calculations + MLOAD.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L133-L140

*Gas Savings as shown via `forge snapshot --diff`*:
```
testFuseCharacterWithSkinToneModifier() (gas: 3 (0.001%))
testFuseWithEmojiSupportToneModifier() (gas: -48 (-0.004%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -38 (-0.007%))
testBurnAsApprovedForAllOfOwner() (gas: -38 (-0.008%))
testFusingAsApprovedOfTray() (gas: -39 (-0.008%))
testFusingWithOneTrayAndOneTile() (gas: -38 (-0.008%))
testBurnAsOwner() (gas: -39 (-0.008%))
testFusingAsOwnerOfTray() (gas: -39 (-0.008%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -96 (-0.009%))
testTokenURIProperlyRendered() (gas: -39 (-0.009%))
testComplexScenario() (gas: -2016 (-0.010%))
testTokenURIOfBurnedToken() (gas: -38 (-0.010%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -48 (-0.011%))
testFuseEmojiDoesNotSupportSkinTone() (gas: -48 (-0.015%))
testFusingWithMuTrayAndMultiTiles() (gas: -192 (-0.018%))
testFusingWithOneTrayAndMultiTiles() (gas: -115 (-0.022%))
testBurnAsApproved() (gas: -269 (-0.040%))
testFusingDuringPrelaunch() (gas: -336 (-0.058%))
Overall gas change: -3473 (-0.002%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
133:            Tray.TileData memory tileData = tray.getTile(trayID, tileOffset); // Will revert if tileOffset is too high
134:            uint8 characterModifier = tileData.characterModifier;
135:
136:            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) {
137:                revert CannotFuseCharacterWithSkinTone();
138:            }
139:            
140:            if (tileData.fontClass == 0) {
141:                // Emoji
142:                characterModifier = _characterList[i].skinToneModifier;
143:            }
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..6c3e961 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -132,12 +132,13 @@ contract Namespace is ERC721, Owned {
             }
             Tray.TileData memory tileData = tray.getTile(trayID, tileOffset); // Will revert if tileOffset is too high
             uint8 characterModifier = tileData.characterModifier;
+            uint8 fontClass = tileData.fontClass;

-            if (tileData.fontClass != 0 && _characterList[i].skinToneModifier != 0) {
+            if (fontClass != 0 && _characterList[i].skinToneModifier != 0) {
                 revert CannotFuseCharacterWithSkinTone();
             }

-            if (tileData.fontClass == 0) {
+            if (fontClass == 0) {
                 // Emoji
                 characterModifier = _characterList[i].skinToneModifier;
             }
```

## Multiple uint/ID mappings can be combined into a single mapping of a uint/ID to a struct, where appropriate
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L46-L49

*Gas Savings as shown via `forge snapshot --diff`*:
```
testFuseEmojiDoesNotSupportSkinTone() (gas: 13 (0.004%))
testFuseCharacterWithSkinToneModifier() (gas: 13 (0.004%))
testComplexScenario() (gas: -1146 (-0.005%))
testFuseWithEmojiSupportToneModifier() (gas: -83 (-0.007%))
testFusingWithDuplicateTiles() (gas: 25 (0.008%))
testSelectTrayIdToFuseSupportSkinTone() (gas: -123 (-0.011%))
testFusingWithMuTrayAndMultiTiles() (gas: -126 (-0.012%))
testFusingAsApprovedForAllOfTrayOwner() (gas: -66 (-0.013%))
testFusingAsApprovedOfTray() (gas: -67 (-0.013%))
testFusingWithOneTrayAndOneTile() (gas: -66 (-0.014%))
testFusingAsOwnerOfTray() (gas: -67 (-0.014%))
testFusingWithOneTrayAndMultiTiles() (gas: -95 (-0.018%))
testFusingWithPrelaunchTrayAfterPrelaunch() (gas: -82 (-0.020%))
testTokenURIProperlyRendered() (gas: -109 (-0.025%))
testBurnAsApprovedForAllOfOwner() (gas: -147 (-0.029%))
testBurnAsOwner() (gas: -147 (-0.031%))
testTokenURIOfBurnedToken() (gas: -129 (-0.033%))
testFusingDuringPrelaunch() (gas: -191 (-0.033%))
testBurnAsApproved() (gas: -234 (-0.035%))
testFusingWithMoreThan13Characters() (gas: 22 (0.141%))
testTokenURIOfNonMintedToken() (gas: 22 (0.175%))
testFusingWith0Characters() (gas: 22 (0.221%))
Overall gas change: -2761 (-0.001%)
```
```solidity
File: canto-namespace-protocol/src/Namespace.sol
46:    mapping(uint256 => string) public tokenToName;
47:
48:    /// @notice Stores the character data of an NFT
49:    mapping(uint256 => Tray.TileData[]) private nftCharacters;
```
```diff
diff --git a/src/Namespace.sol b/src/Namespace.sol
index 484d049..4d8ff12 100644
--- a/src/Namespace.sol
+++ b/src/Namespace.sol
@@ -35,6 +35,10 @@ contract Namespace is ERC721, Owned {
         /// @notice Emoji modifier for the skin tone. Can have values of 0 (yellow) and 1 - 5 (light to dark). Only supported by some emojis
         uint8 skinToneModifier;
     }
+    struct TokenData {
+        string nftName;
+        Tray.TileData[] nftCharacters;
+    }

     /// @notice Next Namespace ID to mint. We start with minting at ID 1
     uint256 public nextNamespaceIDToMint;
@@ -42,11 +46,7 @@ contract Namespace is ERC721, Owned {
     /// @notice Maps names to NFT IDs
     mapping(string => uint256) public nameToToken;

-    /// @notice Maps NFT IDs to (ASCII) names
-    mapping(uint256 => string) public tokenToName;
-
-    /// @notice Stores the character data of an NFT
-    mapping(uint256 => Tray.TileData[]) private nftCharacters;
+    mapping(uint256 => TokenData) public tokenData;

     /*//////////////////////////////////////////////////////////////
                                  EVENTS
@@ -85,18 +85,23 @@ contract Namespace is ERC721, Owned {
         }
     }

+    function tokenToName(uint256 _id) public view returns (string memory) {
+        return tokenData[_id].nftName;
+    }
+
     /// @notice Get the token URI for the specified _id
     /// @param _id ID to query for
     function tokenURI(uint256 _id) public view override returns (string memory) {
         if (_ownerOf[_id] == address(0)) revert TokenNotMinted(_id);
+        TokenData storage _tokenData = tokenData[_id];
         string memory json = Base64.encode(
             bytes(
                 string(
                     abi.encodePacked(
                         '{"name": "',
-                        tokenToName[_id],
+                        _tokenData.nftName,
                         '", "image": "data:image/svg+xml;base64,',
-                        Base64.encode(bytes(Utils.generateSVG(nftCharacters[_id], false))),
+                        Base64.encode(bytes(Utils.generateSVG(_tokenData.nftCharacters, false))),
                         '"}'
                     )
                 )
@@ -113,7 +118,8 @@ contract Namespace is ERC721, Owned {
         uint256 fusingCosts = 2**(13 - numCharacters) * 1e18;
         SafeTransferLib.safeTransferFrom(note, msg.sender, revenueAddress, fusingCosts);
         uint256 namespaceIDToMint = ++nextNamespaceIDToMint;
-        Tray.TileData[] storage nftToMintCharacters = nftCharacters[namespaceIDToMint];
+        TokenData storage _tokenData = tokenData[namespaceIDToMint];
+        Tray.TileData[] storage nftToMintCharacters = _tokenData.nftCharacters;
         bytes memory bName = new bytes(numCharacters * 33); // Used to convert into a string. Can be 33 times longer than the string at most (longest zalgo characters is 33 byte
s)
         uint256 numBytes;
         // Extract unique trays for burning them later on
@@ -169,7 +175,7 @@ contract Namespace is ERC721, Owned {
         uint256 currentRegisteredID = nameToToken[nameToRegister];
         if (currentRegisteredID != 0) revert NameAlreadyRegistered(currentRegisteredID);
         nameToToken[nameToRegister] = namespaceIDToMint;
-        tokenToName[namespaceIDToMint] = nameToRegister;
+        _tokenData.nftName = nameToRegister;

         for (uint256 i; i < numUniqueTrays; ++i) {
             tray.burn(uniqueTrays[i]);
@@ -185,8 +191,9 @@ contract Namespace is ERC721, Owned {
         address nftOwner = ownerOf(_id);
         if (nftOwner != msg.sender && getApproved[_id] != msg.sender && !isApprovedForAll[nftOwner][msg.sender])
             revert CallerNotAllowedToBurn();
-        string memory associatedName = tokenToName[_id];
-        delete tokenToName[_id];
+        TokenData storage _tokenData = tokenData[_id];
+        string memory associatedName = _tokenData.nftName;
+        _tokenData.nftName = "";
         delete nameToToken[associatedName];
         _burn(_id);
     }
@@ -207,3 +214,4 @@ contract Namespace is ERC721, Owned {
         emit RevenueAddressUpdated(currentRevenueAddress, _newRevenueAddress);
     }
 }
```

## Consider creating additional `burn` function in `Tray.sol` to avoid multiple external calls in for loop
Instead of looping through `uniqueTrays` and performing an external call (~100 gas) on each iteration, an additional burn function can be added to `Tray.sol` that will loop through `uniqueTrays` and burn them. This would save 100 * `_characterList.length` (at most 1300) gas for the `fuse` function since only one external call is needed.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L174-L176

```solidity
File: canto-namespace-protocol/src/Namespace.sol
174:        for (uint256 i; i < numUniqueTrays; ++i) {
175:            tray.burn(uniqueTrays[i]);
176:        }
```

**With the addition of a new burn function the above code can be refactored into something like this:**
```solidity
tray.burnTrays(uniqueTrays);
```

## Consider using `bytes32` to set variable as immutable and save an SLOAD
The `subprotocolName` variable is only set once in the constructor and therefore can be set to immutable to save ~2100 gas (`Gcoldsload`). However, variables of type `string` can not be set as immutable. If it is reasonable to enforce `subprotocolName` to always be <= 32 bytes, then `bytes32` can be used instead of `string` which will allow the variable to be set to `immutable`.

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L35

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L99

```solidity
35:        string public subprotocolName;

99:        uint256 cidNFTID = cidNFT.getPrimaryCIDNFT(subprotocolName, _pfpID);
```
