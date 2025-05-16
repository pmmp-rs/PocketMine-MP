# PocketMine-MP TODO Statements and PM6 Changes

## TODO Statements in the Codebase

This document lists the TODO statements found in the PocketMine-MP codebase, organized by category. These represent areas that may need future improvement or completion.

### World System

#### src/world/World.php
- `public const DEFAULT_TICKED_BLOCKS_PER_SUBCHUNK_PER_TICK = 3;`
  - TODO: "this could probably do with being a lot bigger"
- In chunk ticking configuration:
  - TODO: "this needs l10n" (regarding the "chunk-ticking.per-tick" setting deprecation warning)
- In random tick blocks configuration:
  - TODO: "this is a really sketchy hack - remove this as soon as possible" (regarding block state upgrading from integer IDs)
- In light population task:
  - TODO: "phpstan can't infer these types yet :(" (regarding type annotations for light arrays)
  - TODO: "calculated light information might not be valid if the terrain changed during light calculation"
- TODO: "Accept plain integers in PM6 - the Vector3 requirement is an unnecessary inconvenience"
- TODO: "make this the primary method in PM6" (regarding certain world methods)

#### src/world/format/io/data/BedrockWorldData.php
- In `fix` method:
  - TODO: "add a null generator which does not generate missing chunks" (to allow importing back to MCPE)

#### src/world/light/LightUpdate.php
- TODO: "make light updates asynchronous"

### Entity System

#### src/entity/Entity.php
- In `entityBaseTick` method:
  - TODO: "check vehicles"
- TODO: "hack for client-side AI interference: prevent client sided movement when motion is 0"
- TODO: "We should be setting FLAG_TELEPORT here to disable client-side movement interpolation"
- TODO: "vehicle collision events (first we need to spawn them!)"

#### src/entity/Human.php
- TODO: "use of NIL UUID for namespace is a hack; we should provide a proper UUID for the namespace"
- TODO: "head yaw" (regarding packet data)

#### src/entity/Living.php
- TODO: "load/save armor inventory contents"
- TODO: "knockback should not just apply for entity damage sources"
- TODO: "check death conditions (must have been damaged by player < 5 seconds from death)"

### Block System

#### src/block/tile/Sign.php
- In `addAdditionalSpawnData` method:
  - TODO: "not sure what this is used for" (regarding the `PERSIST_FORMATTING` tag)

#### src/block/PressurePlate.php
- TODO: "make this mandatory in PM6" (regarding deactivationDelayTicks parameter)
- TODO: "make this abstract in PM6" (regarding certain methods)

#### src/block/utils/FallableTrait.php
- TODO: "convert this into a dynamic component"

#### src/block/utils/BlockEventHelper.php
- TODO: "try to further reduce the amount of code duplication here"

#### src/block/RuntimeBlockStateRegistry.php
- TODO: "We'll want to redesign block collision box handling and block shapes in the future"

### Permission System

#### src/permission/PermissibleInternal.php
- In `addAttachment` method:
  - TODO: "tick scheduled attachments"

### Network and Protocol

#### src/network/mcpe/handler/InGamePacketHandler.php
- Multiple TODOs regarding packet handling, including:
  - TODO: "this packet has WAYYYYY more useful information that we're not using"
  - TODO: "HACK: EATING_ITEM is sent back to the server when the server sends it for other players (1.14 bug, maybe earlier)"
  - TODO: "start/end hack for client spam bug"

#### src/network/mcpe/NetworkSession.php
- TODO: "make player data loading async"
- TODO: "we shouldn't be loading player data here at all, but right now we don't have any choice :("
- TODO: "HACK: fix client-side falling pre-spawn"

### Performance Monitoring

#### src/timings/TimingsHandler.php
- `printTimings` method is deprecated with note:
  - "This only collects timings from the main thread. Collecting timings from all threads is an async operation, so it can't be done synchronously."

### Data Conversion and Serialization

#### src/data/bedrock/block/convert/BlockStateDeserializerHelper.php
- TODO: "check if these need any special treatment to get the appropriate data to both halves of the door"
- TODO: "not sure what the point of this is" (regarding ITEM_FRAME_PHOTO_BIT)

#### src/data/bedrock/item/ItemDeserializer.php
- TODO: "this is rough duct tape; we need a better way to deal with this"
- TODO: "worth caching this or not?"
- TODO: "canDestroy, canPlaceOn, wasPickedUp are currently unused"

### Build System

#### build/dump-version-info.php
- In options array:
  - TODO: "maybe this should be put into its own script?" (regarding suffix validation)

#### build/generate-build-info-json.php
- In JSON encoding:
  - TODO: "maybe we should embed this in VersionInfo?" (regarding the date field)

## PM6 Changes and Plans

Based on the codebase analysis, here are the known changes and plans for PocketMine-MP version 6 (PM6):

### Explicitly Mentioned PM6 Changes

#### Block and Item System
- **VanillaBlocks and VanillaItems Redesign** (mentioned in changelogs/5.23.md)
  - The team is exploring options to redesign `VanillaBlocks` and `VanillaItems` to eliminate the need for defining separate TypeIds entirely
  - This follows improvements in PM5 that significantly improved readability of these classes and eliminated code like `WoodLikeBlockIdHelper`

#### Enum Handling
- **LegacyEnumShimTrait Removal**
  - Many enum-like classes have TODOs indicating that tags need to be removed once LegacyEnumShimTrait is removed in PM6
  - Affected classes include:
    - Block utils: BannerPatternType, BellAttachmentType, BrewingStandSlot, CopperOxidation, CoralType, DirtType, DripleafState, DyeColor, FroglightType, LeavesType, LeverFacing, MobHeadType, MushroomBlockType, RecordType, SaplingType, SlabType, StairShape, SupportType, WallConnectionType, WoodType
    - Item related: BoatType, ItemUseResult, MedicineType, PotionType, SuspiciousStewType, ToolTier
    - Other: FurnaceType, GameMode, NoteInstrument, PluginEnableOrder, ShapelessRecipeType, TreeType, UsedChunkStatus

#### World System
- **Vector3 Requirements**
  - TODO in World.php: "Accept plain integers in PM6 - the Vector3 requirement is an unnecessary inconvenience"
  - TODO in World.php: "make this the primary method in PM6" (regarding certain world methods)

#### Block System
- **PressurePlate Changes**
  - TODO: "make deactivationDelayTicks parameter mandatory in PM6"
  - TODO: "make certain methods abstract in PM6"

### Dependency Requirements
While not explicitly labeled as PM6 changes, the following dependency requirements may be relevant for the upcoming version:

- **PHP Requirements**:
  - Current minimum PHP version is 8.1
  - PHP must be 64-bit
  - IPv6 support is required

- **PHP Extensions**:
  - pmmpthread ^6.1.0 (renamed from pthreads)
  - leveldb ^0.2.1 or ^0.3.0
  - Various other extensions including chunkutils2, crypto, gmp, igbinary, morton, etc.

### Additional Potential Areas for PM6 Changes
Based on TODOs and recent changes, these areas might see improvements in PM6:

1. **API Improvements**
   - Continued refinement of plugin APIs
   - Potential changes to permission systems
   - More consistent method signatures and parameter requirements

2. **Performance Optimizations**
   - Improvements to block ticking
   - Enhanced memory usage and processing efficiency
   - Better async processing for light updates and other operations

3. **World Generation and Handling**
   - Potential improvements to chunk ticking and light calculation
   - Better handling of world data conversion

Note: This document is based on the current state of the codebase and may not reflect all planned changes for PM6, as many may not yet be documented in the code.
