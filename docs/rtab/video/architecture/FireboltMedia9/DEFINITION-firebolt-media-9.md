# Firebolt Media 9 — Client API Definition

**Document version:** 0.1

**Status:** Draft

## 1. Purpose

Firebolt Media 9 retains the Firebolt Media 8 client API. It adds video texturing for applications that need it, playback-quality measurements, playback identification, persistent-license updates and batch decryption control. It also adds audio presentation delay to help compensate for delayed display when using video textures.

Section 2 lists the changes from Firebolt Media 8. Sections 3 and 4 summarize the retained Firebolt Media 8 API and playback behavior. Sections 5–9 define the Firebolt Media 9 additions and changes in detail.

C++ declarations retain the existing namespace:

```cpp
namespace firebolt::rialto
{
}
```

The declaration excerpts below show members of the named interfaces. Parameter types and names follow the client headers. The detailed additions state their time units explicitly.

## 2. Deltas from Firebolt Media 8

Firebolt Media 9 adds the following API features and behavior:

- **Video texturing.** Adds an optional pipeline output type for applications that need video as GL textures or Vulkan images. Applications select this output through typed capture objects and pipeline attachment and detachment operations. Video-plane output and `setVideoWindow()` remain supported. See §5.
- **Media metrics.** Adds `setMetricSubscription()`, `notifyMetric()` and `notifyEventsMissed()` for selecting measurements, receiving observations and detecting lost notifications. See §§6.1–6.4.
- **Video frame counters.** Adds `getFrameCounters()` with rendered, dropped and corrupted frame counts. The existing `getStats()` remains supported. See §6.5.
- **Playback identification.** Adds `setPlaybackSessionId()` to associate measurements with an application-provided playback UUID. See §6.7.
- **Decode-error reporting deprecation.** Deprecates `setReportDecodeErrors()` in favor of selecting decode-error metrics. The existing operation retains its behavior. See §6.6.
- **Persistent-license restore.** Adds a `loadSession()` overload that accepts a saved persistent session identifier. See §7.2.
- **Persistent-license removal.** Adds `onLicenseRelease()` and defines the release-message and acknowledgement workflow using `removeKeySession()` and `updateSession()`. See §7.3.
- **Key-status values.** Adds `OUTPUT_DOWNSCALED` and `USABLE_IN_FUTURE`, and reports downscaled output using its dedicated status. See §7.4.
- **Batch decryption control.** Adds pipeline-level `setBatchDecryption()`, disabled by default. See §8.
- **Audio presentation delay.** Adds signed pipeline-level `setAudioDelay()` and `getAudioDelay()` to help compensate for delayed display when using video textures. See §9.

## 3. Firebolt Media 8 API overview

### 3.1 Create playback and describe media

`IMediaPipelineFactory::createFactory()` obtains a factory. Its `createMediaPipeline()` creates a pipeline using an `IMediaPipelineClient` callback object and `VideoRequirements`.

`VideoRequirements::maxWidth` and `maxHeight` describe the maximum video dimensions needed by the application.

| Application task | Interface or operation |
|---|---|
| Prepare playback | `IMediaPipeline::load()` |
| Describe audio, video or subtitles | `MediaSource`, `MediaSourceAV`, `MediaSourceAudio`, `MediaSourceVideo`, `MediaSourceVideoDolbyVision`, `MediaSourceSubtitle` |
| Attach or remove a source | `attachSource()`, `removeSource()` |
| Finish attaching the source set | `allSourcesAttached()` |
| Switch the audio source | `switchSource()` |
| Retrieve the callback object | `getClient()` |

Source descriptions carry the media type, MIME type, configuration and encryption information. Audio configuration includes channel count, sample rate and codec data. Source identifiers returned through successful attachment identify the sources in later calls and callbacks.

### 3.2 Supply media and control playback

| Application task | `IMediaPipeline` operations |
|---|---|
| Supply requested media | `addSegment()`, then `haveData()` |
| Start, pause or stop | `play()`, `pause()`, `stop()` |
| Set playback speed | `setPlaybackRate()` |
| Seek or read position | `setPosition()`, `getPosition()` |
| Read duration | `getDuration()` |
| Flush one source | `flush()` |
| Set a source's starting position | `setSourcePosition()` |
| Display a prepared video frame | `renderFrame()` |
| Read queued frames | `getQueuedFrames()` |
| Control buffering | `setBufferingLimit()`, `getBufferingLimit()`, `setUseBuffering()`, `getUseBuffering()` |

Applications provide media using `MediaSegment`, `MediaSegmentAudio` and `MediaSegmentVideo`. Segments describe the source, timing, media bytes and any encryption information needed for that sample.

### 3.3 Control audio, video and subtitles

| Application task | `IMediaPipeline` operations |
|---|---|
| Position video-plane output | `setVideoWindow()` |
| Change or read volume | `setVolume()`, `getVolume()` |
| Mute or unmute a source | `setMute()`, `getMute()` |
| Select a text track | `setTextTrackIdentifier()`, `getTextTrackIdentifier()` |
| Adjust subtitle timing | `setSubtitleOffset()` |
| Control immediate output | `setImmediateOutput()`, `getImmediateOutput()` |
| Select low-latency behavior | `setLowLatency()` |
| Control synchronization | `setSync()`, `getSync()`, `setSyncOff()`, `setStreamSyncMode()`, `getStreamSyncMode()` |
| Describe an audio gap | `processAudioGap()` |

`setVolume()` accepts a level from `0.0` to `1.0`, an optional transition duration in milliseconds and an easing type. `setMute()` addresses a source by its identifier.

### 3.4 Handle playback callbacks

Applications supply an `IMediaPipelineClient` object.

| Information or request | Callbacks |
|---|---|
| Timing and video size | `notifyDuration()`, `notifyPosition()`, `notifyNativeSize()` |
| Network and playback state | `notifyNetworkState()`, `notifyPlaybackState()`, `notifyPlaybackInfo()` |
| Audio/video data availability | `notifyAudioData()`, `notifyVideoData()` |
| Requests for media | `notifyNeedMediaData()`, `notifyCancelNeedMediaData()` |
| Existing quality and error information | `notifyQos()`, `notifyBufferUnderflow()`, `notifyFirstFrameReceived()`, `notifyPlaybackError()` |
| Source-flush completion | `notifySourceFlushed()` |

Duration and position use nanoseconds. `kDurationUnknown` identifies an unknown duration; `kDurationUnending` identifies an unending stream. `notifyFirstFrameReceived()` reports receipt of the first frame. Applications use it according to that meaning rather than as proof of display.

### 3.5 Play Web Audio

`IWebAudioPlayerFactory::createFactory()` obtains a factory. `createWebAudioPlayer()` takes an `IWebAudioPlayerClient`, audio MIME type, priority and `WebAudioConfig`. The Firebolt Media 8 format is raw PCM audio (`audio/x-raw`).

| Application task | `IWebAudioPlayer` operations |
|---|---|
| Start or pause audio | `play()`, `pause()` |
| Find preferred and maximum write sizes | `getDeviceInfo()` |
| Check available space | `getBufferAvailable()` |
| Supply audio frames | `writeBuffer()` |
| Read the number of frames waiting to play | `getBufferDelay()` |
| Signal that no more audio will be supplied | `setEos()` |
| Control volume | `setVolume()`, `getVolume()` |
| Retrieve the callback object | `getClient()` |

Use `getDeviceInfo()` to determine whether audio can be supplied before `play()`. Write no more frames than the available space. `IWebAudioPlayerClient::notifyState()` reports `WebAudioPlayerState` changes.

Priority `1` is the most important. On a platform that can mix only a limited number of players, lower-priority players beyond that limit may accept data while their audio is discarded.

### 3.6 Manage protected media

`IMediaKeysFactory::createFactory()` obtains a factory. `createMediaKeys(keySystem)` creates an `IMediaKeys` object for the selected key system.

| Application task | `IMediaKeys` operations |
|---|---|
| Create a key session | `createKeySession()` |
| Request a license | `generateRequest()` |
| Supply a license response | `updateSession()` |
| Load an existing live session | `loadSession(keySessionId)` |
| Select or test a key | `selectKeyId()`, `containsKey()` |
| Supply a DRM header | `setDrmHeader()` |
| Close, remove or release a session handle | `closeKeySession()`, `removeKeySession()`, `releaseKeySession()` |
| Read a persistent identifier | `getCdmKeySessionId()` |
| Read diagnostics or system data | `getLastDrmError()`, `getDrmTime()`, `getMetricSystemData()` |
| Read storage hashes and session limits | `getDrmStoreHash()`, `getKeyStoreHash()`, `getLdlSessionsLimit()` |
| Clear DRM or key storage | `deleteDrmStore()`, `deleteKeyStore()` |

`IMediaKeysClient` receives `onLicenseRequest()`, `onLicenseRenewal()` and `onKeyStatusesChanged()`.

`KeySessionType` provides `TEMPORARY`, `PERSISTENT_LICENCE` and `PERSISTENT_RELEASE_MESSAGE`.

### 3.7 Query capabilities

Obtain each factory with its `createFactory()` operation. Use `createMediaPipelineCapabilities()`, `getMediaKeysCapabilities()` or `createMediaCapabilities()` to obtain the corresponding query interface.

| Factory and interface | Application queries |
|---|---|
| `IMediaPipelineCapabilitiesFactory`, `IMediaPipelineCapabilities` | `getSupportedMimeTypes()`, `isMimeTypeSupported()`, `getSupportedProperties()`, `isVideoMaster()` |
| `IMediaKeysCapabilitiesFactory`, `IMediaKeysCapabilities` | `getSupportedKeySystems()`, `supportsKeySystem()`, `getSupportedKeySystemVersion()`, `isServerCertificateSupported()`, `getSupportedRobustnessLevels()` |
| `IMediaCapabilitiesFactory`, `IMediaCapabilities` | `getSupportedAudioCapabilities()`, `getSupportedVideoCapabilities()` |

### 3.8 Observe application state and receive logs

`IControlFactory::createFactory()` and `createControl()` provide `IControl`. `registerClient()` registers an `IControlClient` and returns the current `ApplicationState`: `UNKNOWN`, `RUNNING` or `INACTIVE`. Later changes arrive through `notifyApplicationState()`.

`IClientLogControlFactory::createFactory()` and `createClientLogControl()` provide log control. `registerLogHandler()` registers an `IClientLogHandler`; its `log()` callback receives the level, source location, function and message. These interfaces retain Firebolt Media 8 behavior.

## 4. Firebolt Media 8 playback and media delivery

### 4.1 Normal setup

1. Create the pipeline callback object and factory.
2. Create a pipeline with the required maximum video dimensions. Check the returned pointer.
3. Call `load()` and check its result.
4. Attach the audio, video and subtitle sources needed for playback. Save their source identifiers.
5. Call `allSourcesAttached()` and check its result.
6. Call `pause()` to prepare playback, or `play()` to start it. Handle state and media-data callbacks.

`allSourcesAttached()` succeeds once per pipeline object. Repeating `load()` does not allow a second successful source-set setup. Create a new pipeline when a new source set is required.

Setup completes asynchronously. `IDLE` is reported after the first `pause()` or `play()` begins setup; a successful `allSourcesAttached()` call alone is not an `IDLE` notification.

### 4.2 Respond to media requests

For each `notifyNeedMediaData()` request:

1. Use its `sourceId` and `needDataRequestId` to identify the requested data.
2. Call `addSegment()` for each sample being supplied, within the requested limits.
3. Call `haveData()` with the same request identifier and the appropriate `MediaSourceStatus`.

If `addSegment()` returns `NO_SPACE`, retain the unaccepted sample and call `haveData()` for the samples already accepted. Retry the retained sample when the next data request arrives.

`MediaSourceStatus::EOS` marks the end of that source. Handle `notifyCancelNeedMediaData()` by cancelling outstanding work for the identified source.

### 4.3 Public playback states

| `PlaybackState` | Application meaning |
|---|---|
| `UNKNOWN` | Setup has not yet produced a public playback-state notification. |
| `IDLE` | Initial source setup has completed. |
| `PAUSED` | Playback is paused. |
| `PLAYING` | Playback is running. |
| `SEEKING` | A seek is in progress. |
| `SEEK_DONE` | The flushing seek call succeeded; preparation of the new media follows. |
| `END_OF_STREAM` | Playback reached the end of the stream. |
| `STOPPED` | Playback has stopped. |
| `FAILURE` | The pipeline has failed. |

Use `notifyPlaybackState()` to follow state changes. `setPosition()` seeks and clears queued media and outstanding requests. After `SEEK_DONE`, playback prepares the new position and returns to `PAUSED`, then to `PLAYING` if playback was running.

`flush(sourceId, ...)` clears one source while preserving playback state. Use `notifySourceFlushed(sourceId)` to observe its completion. `setSourcePosition()` also addresses one source and preserves playback state.

### 4.4 Results and completion

A successful call must be read according to that operation's contract. Playback commands can complete asynchronously; their acceptance alone does not show that the requested state or playback rate has taken effect.

## 5. Video texturing through frame capture

### 5.1 Output model

Applications that need video texturing use the frame-capture API to receive a pipeline's video as GL textures or Vulkan images. The application can then use those resources in its own graphics and display them.

This is an additional pipeline output type. Pipelines using video-plane output retain their existing behavior and `setVideoWindow()` control. When an application attaches a capture, that pipeline delivers video through the capture rather than the video plane.

Each pipeline supports one video source and one capture. Each capture is attached to at most one pipeline at a time. Separate pipelines can provide one capture output and one video-plane output.

Capture supports clear content and 8-bit standard-dynamic-range output. `supportsProtectedContent` and `supportsHdr` are both `0`. Higher-bit-depth sources are reduced to 8-bit output by the capture path.

### 5.2 Create a typed capture

`IVideoFrameCaptureFactory` provides:

```cpp
static std::shared_ptr<IVideoFrameCaptureFactory> createFactory();

virtual std::shared_ptr<IGLVideoFrameCapture>
    createGLVideoFrameCapture(EGLDisplay eglDisplay, EGLContext eglContext) = 0;

virtual std::shared_ptr<IVulkanVideoFrameCapture>
    createVulkanVideoFrameCapture(VkPhysicalDevice physicalDevice, VkDevice device,
                                 VkQueue queue, VkImageLayout requiredImageLayout) = 0;
```

Check the factory and capture pointers before use. Creation returns null when capture is unavailable or creation fails.

The graphics API is fixed at creation. `IVideoFrameCapture::apiType()` returns `GraphicsAPIType::GL` or `GraphicsAPIType::VULKAN`. A GL capture returns GL frames; a Vulkan capture returns Vulkan frames.

Keep the supplied EGL display and contexts sharing the supplied graphics resources, or the Vulkan device and queue, valid until all frames have been released and the capture has been destroyed.

### 5.3 Attach and configure capture

`IMediaPipeline` adds:

```cpp
virtual bool attachVideoFrameCapture(
    const std::shared_ptr<IVideoFrameCapture> &videoFrameCapture);
virtual bool detachVideoFrameCapture();
```

Application setup order:

1. Create the pipeline and call `load()` successfully.
2. Create the capture for the application's graphics resources.
3. Attach the capture before `allSourcesAttached()`.
4. Attach a clear video source. Source attachment may occur before or after capture attachment.
5. Call `allSourcesAttached()` and check its result.
6. Query capture capabilities after successful setup, then prepare or start playback.

`attachVideoFrameCapture()` returns `false` at an invalid time, for a null or unsuitable capture, when a capture is already attached, or when the feature is unsupported. The pipeline retains a reference to the attached capture.

With capture attached, `allSourcesAttached()` checks the video source, declared maximum dimensions and capture support before returning success. A setup failure returns `false` and leaves source setup available for another attempt. Correct the configuration or detach capture before retrying.

Successful capture setup does not change the asynchronous playback preparation described in §4.1. Without capture attached, source setup retains Firebolt Media 8 behavior.

`setVideoWindow()` returns `false` while capture is attached. Use application graphics to position capture output.

`detachVideoFrameCapture()` is valid after `load()` and before successful `allSourcesAttached()`, or in `STOPPED`. Detachment during playback returns `false`. Pipeline destruction detaches automatically.

A capture can be reused with a new pipeline after it has been released from the previous pipeline. Frames already acquired remain valid across stop, detachment and pipeline destruction while the application retains them.

### 5.4 Query capabilities

The typed capture interfaces provide:

```cpp
virtual VideoFrameCaptureStatus getCapabilities(
    GLVideoFrameCaptureCaps &out, VideoFrameCaptureStatus &status) const = 0;
```

```cpp
virtual VideoFrameCaptureStatus getCapabilities(
    VulkanVideoFrameCaptureCaps &out, VideoFrameCaptureStatus &status) const = 0;
```

The first declaration belongs to `IGLVideoFrameCapture`; the second belongs to `IVulkanVideoFrameCapture`. Both the return value and the `status` output parameter carry the query result. Use the capability data after a successful query.

Query after the capture is attached and `allSourcesAttached()` has completed successfully. An earlier query returns `SESSION_NOT_READY`.

```cpp
struct VideoFrameCaptureCaps
{
    uint32_t size{sizeof(VideoFrameCaptureCaps)};
    uint32_t maxConcurrentFrames;
    uint32_t maxResourcesPerFrame;
    uint32_t supportsProtectedContent;
    uint32_t supportsHdr;
    uint32_t reserved_0;
};

struct GLVideoFrameCaptureCaps
{
    uint32_t size{sizeof(GLVideoFrameCaptureCaps)};
    VideoFrameCaptureCaps common;
    uint32_t reserved_0;
    uint32_t reserved_1;
};

struct VulkanVideoFrameCaptureCaps
{
    uint32_t size{sizeof(VulkanVideoFrameCaptureCaps)};
    VideoFrameCaptureCaps common;
    std::vector<VkFormat> supportedFormats;
    uint32_t reserved_0;
};
```

| Field | Meaning |
|---|---|
| `size` | Structure size used by the query. |
| `maxConcurrentFrames` | Graphics-support ceiling for simultaneously retained frames; setup selects the supported operating capacity within that ceiling. |
| `maxResourcesPerFrame` | Maximum number of textures or image resources exposed for one frame. |
| `supportsProtectedContent` | `0` in Firebolt Media 9. |
| `supportsHdr` | `0` in Firebolt Media 9. |
| `supportedFormats` | Vulkan formats supported by the capture. |
| `reserved_0`, `reserved_1` | Reserved fields; set to zero. |

`VideoFrameCaptureStatus` retains this declaration order:

```cpp
enum class VideoFrameCaptureStatus
{
    OK,
    NO_NEW_FRAME,
    NO_FRAME,
    UNSUPPORTED,
    INVALID_ARGUMENT,
    NO_RESOURCES,
    FATAL_ERROR,
    SESSION_NOT_READY
};
```

`acquireCurrentFrame()` exposes its ordinary results through `changed` and `frame`, as described next; it does not return this status enum.

### 5.5 Acquire the current frame

```cpp
struct GLFrameAcquisition
{
    bool changed;
    std::shared_ptr<IGLVideoFrame> frame;
};

struct VulkanFrameAcquisition
{
    bool changed;
    std::shared_ptr<IVulkanVideoFrame> frame;
};
```

`IGLVideoFrameCapture::acquireCurrentFrame()` returns `GLFrameAcquisition`. `IVulkanVideoFrameCapture::acquireCurrentFrame()` returns `VulkanFrameAcquisition`.

Each call observes the current frame selected for presentation by the platform video output. It uses the same timing as video-plane presentation and does not wait for decoding or presentation of another frame.

| Result | `changed` | `frame` | Application action |
|---|---|---|---|
| Newly selected frame | `true` | New shared frame object | Update graphics resources and retain the frame through graphics completion. |
| Same selection as before | `false` | Current shared frame object | Continue using the current frame; retain it for any new graphics work. |
| No current selection | `false` | Null | Wait for a later acquisition; retain older frames only for work still using them. |

A distinct selection is returned with `changed == true` at most once. Repeated unchanged acquisitions share the same frame object. If the application acquires more slowly than video advances, intermediate frames are skipped.

For GL, call acquisition with a context sharing the configured graphics resources current on the calling thread. For Vulkan, keep the configured device valid.

### 5.6 Read frame properties

All frames provide these `IVideoFrame` members:

```cpp
virtual uint32_t frameIndex() const = 0;
virtual uint32_t numResources() const = 0;
virtual GraphicsTextureLayout layout() const = 0;
virtual int64_t presentationTimeStampNs() const = 0;
virtual uint32_t textureWidth() const = 0;
virtual uint32_t textureHeight() const = 0;
virtual uint32_t visibleWidth() const = 0;
virtual uint32_t visibleHeight() const = 0;
```

| Property | Application meaning |
|---|---|
| `frameIndex()` | Reusable backing index. Use `changed` to detect a new selection rather than treating this index as a unique frame identifier. |
| `numResources()` | Number of textures or images making up the picture. |
| `layout()` | How those resources represent colour and alpha. |
| `presentationTimeStampNs()` | Selected frame's presentation timestamp in nanoseconds. |
| `textureWidth()`, `textureHeight()` | Backing dimensions, including padding. |
| `visibleWidth()`, `visibleHeight()` | Current visible content dimensions. |

`GraphicsTextureLayout::RGBA` uses one resource: RGB for colour and A for alpha. `RGBA_SEPARATE_ALPHA` uses two resources: resource 0 RGB for colour and resource 1 R for alpha.

GL frames expose:

```cpp
virtual uint32_t textureTarget() const = 0;
virtual const std::vector<uint32_t> &textureIds() const = 0;
```

Vulkan frames expose:

```cpp
struct VulkanImageResource
{
    VkImage image;
    VkImageView imageView;
};

virtual VkFormat imageFormat() const = 0;
virtual VkImageLayout imageLayout() const = 0;
virtual const std::vector<VulkanImageResource> &images() const = 0;
```

The returned textures and images provide correctly interpreted 8-bit RGB(A) output. Read dimensions from each frame: visible dimensions can change as streaming quality changes, while backing dimensions may remain larger.

### 5.7 Retain and release frames safely

Holding a shared frame reference keeps its picture available. Keep that reference until every graphics command reading the frame has completed.

Releasing the last reference allows the middleware to reuse the backing for another frame. Releasing a reference does not wait for queued graphics commands. Copying only a texture or image identifier does not keep its contents available.

The application must:

1. Retain a frame reference for each graphics submission using that frame, including unchanged acquisitions.
2. Observe completion of that graphics work using the application's graphics API.
3. Release the associated frame references after completion.
4. Release all frames and destroy the capture before destroying the graphics resources supplied at creation.

For GL, keep the appropriate context current when releasing resources that require GL cleanup. Retained frames keep their original picture and interpretation across supported resolution and colour changes.

Retaining every available frame does not stall decoding, audio or the playback clock. New capture frames are omitted until capacity becomes available; acquisition continues to return the current frame unchanged. Omitted frames are skipped rather than replayed later.

When using video textures, application graphics and display can add delay. Audio/video synchronization through that path is best effort. Applications can use the audio-delay control in §9 to help compensate for the delayed display.

### 5.8 Behavior during playback changes

| Playback point | Capture behavior |
|---|---|
| Before source setup or first selected frame | Acquisition returns a null frame. |
| `PAUSED` | Returns the prepared frame once selected, then the same frame unchanged. |
| `PLAYING` | Returns the current selected frame. |
| `SEEKING` / `SEEK_DONE` | Current selection is cleared; acquisition returns null until a new frame is selected. Previously retained frames remain valid. |
| Video-source flush | Clears the current selection until a new frame is selected. |
| `END_OF_STREAM` | Last selected frame remains current. |
| `STOPPED`, `FAILURE`, detachment or pipeline destruction | New delivery ends. Acquisition returns null; previously retained frames remain valid through their final use. |

Attachment and detachment timing is defined in §5.3. Capture continues while its pipeline exists; when media stops, no new frames are produced.

A supported resolution or backing change preserves the total retention capacity established at setup, including older retained frames. A change that cannot be supported ends capture for that session while older retained frames remain valid and normal playback continues. Create a new pipeline for a source configuration outside the declared requirements.

## 6. Media metrics and playback identification

### 6.1 Select measurements

`IMediaPipeline` adds:

```cpp
virtual bool setMetricSubscription(const std::vector<MetricType> &types);
```

Every pipeline starts with an empty subscription. Select the complete set of types the application wants to receive. An empty vector disables metric delivery.

A successful call replaces the previous set atomically and returns only after the new set is effective. An unknown or duplicate type, `FAILURE`, communication failure or unsupported middleware returns `false` and leaves the previous set effective.

Selection is valid before or after `load()` and in every state except `FAILURE`. It persists across `load()`. All metric types are required; there is no partial subscription success or separate capability query.

```cpp
enum class MetricType : uint32_t
{
    VIDEO_FRAME_DROP = 1,
    AUDIO_FRAME_DROP = 2,
    VIDEO_DECODE_ERROR = 3,
    AUDIO_DECODE_ERROR = 4,
    VIDEO_FRAME_REPEAT = 5,
    AUDIO_GAP = 6,
    BUFFER_UNDERFLOW = 7
};

enum class MetricPhase : uint32_t
{
    OCCURRENCE,
    STARTED,
    RESOLVED
};
```

| Type | Meaning | Notification form |
|---|---|---|
| `VIDEO_FRAME_DROP` | A frame eligible for presentation was discarded, including before decoding or after missing its display deadline. | One `OCCURRENCE` per affected frame. |
| `AUDIO_FRAME_DROP` | Decoded audio was discarded by the decoder or arrived too late for output. | One `OCCURRENCE` per affected frame or sample. |
| `VIDEO_DECODE_ERROR` | Decoder failure or corruption affecting a video frame. | One `OCCURRENCE` per affected frame. |
| `AUDIO_DECODE_ERROR` | Decoder failure or corruption affecting an audio frame. | One `OCCURRENCE` per affected frame. |
| `VIDEO_FRAME_REPEAT` | Previous frame reused because the next frame was unavailable. | `STARTED`, then `RESOLVED`. |
| `AUDIO_GAP` | Output substituted for missing decoded audio. | `STARTED`, then `RESOLVED`. |
| `BUFFER_UNDERFLOW` | Decoder has insufficient input data. | `STARTED`, then `RESOLVED`. |

Frames removed by seek, source flush, end-of-stream cleanup or source reconfiguration are excluded from video drops. Regular frame repetition used to match display cadence is excluded from repeat observations. Silence present in the content is excluded from audio gaps.

### 6.2 Receive observations

`IMediaPipelineClient` adds:

```cpp
virtual void notifyMetric(const MetricEvent &metric);
virtual void notifyEventsMissed(uint32_t count);
```

```cpp
struct MetricEvent
{
    MetricType type{MetricType::VIDEO_FRAME_DROP};
    MetricPhase phase{MetricPhase::OCCURRENCE};
    int32_t sourceId{-1};
    std::optional<int64_t> presentationTimeStampNs;
    std::optional<uint64_t> count;
    std::optional<uint64_t> durationNs;
    uint64_t observedAtMonotonicNs{0};
    int64_t observedAtUnixNs{0};
};
```

Every event identifies its type, phase and attached source. Both observation timestamps record when the middleware accepted the observation: monotonic time and Unix time, in nanoseconds.

`presentationTimeStampNs`, when present, identifies the affected stream position. For a single occurrence, it is the affected frame/sample timestamp when known, otherwise the best current stream timestamp. Multiple observations can therefore have the same timestamp.

For a repeated-frame or audio-gap interval, the stream timestamp identifies the repeated or substituted frame. Optional values can be zero; presence is separate from value.

| Types | Phase | Required additional fields | Optional field |
|---|---|---|---|
| Drops and decode errors | `OCCURRENCE` | None | `presentationTimeStampNs` |
| Video repeat and audio gap | `STARTED` | None | `presentationTimeStampNs` |
| Video repeat and audio gap | `RESOLVED` | `count`, `durationNs` | `presentationTimeStampNs` |
| Buffer underflow | `STARTED` | None | `presentationTimeStampNs` |
| Buffer underflow | `RESOLVED` | `durationNs` | `presentationTimeStampNs` |

For video repeat and audio gap, `count` is the number of repeated or substituted frames. `durationNs` is the interval duration. Other field combinations are discarded without affecting playback.

Notifications arrive on the normal pipeline-callback thread, in acceptance order for that pipeline. Override these callbacks to consume the selected metrics.

### 6.3 Aggregate observations and handle loss

- Count each `OCCURRENCE` as one. Those events carry no count field.
- Use a `RESOLVED` payload as the complete summary of an interval, even if its `STARTED` event was not received.
- A single repeated frame still produces a start and end pair.
- `notifyEventsMissed(count)` reports lost subscribed notifications and arrives before the following metric notifications.
- After loss, mark occurrence-derived totals for that interval as incomplete. Keep any active interval tracking; a later `RESOLVED` remains usable.
- Intentionally filtered types do not contribute to the missed count.

### 6.4 Change subscriptions during playback

| Change | Effect |
|---|---|
| Keep an interval type selected | Its active interval continues. |
| Remove an active interval type | That subscription's interval ends without a later `RESOLVED`. |
| Add an interval type while its condition is active | Starts measurement at enablement and measures only the selected interval. |
| Add an occurrence type | Delivery begins with the next observation. |

No historical observations are recreated. Pending notifications for removed types are discarded where possible; a callback already executing can finish.

Seek and source flush end affected intervals with a boundary `RESOLVED`. Frames removed by those operations are excluded from drops. Underflow is not started while seek is preparing the new decode position.

At `STOPPED`, observation ends after final interval resolutions. At `FAILURE`, observation ends and subscription changes fail. Setting a subscription in `STOPPED` is accepted but produces no observations there.

### 6.5 Read exact video counters

`IMediaPipeline` adds:

```cpp
virtual bool getFrameCounters(int32_t sourceId, VideoFrameCounters &stats) const;
```

```cpp
struct VideoFrameCounters
{
    uint32_t size{sizeof(VideoFrameCounters)};
    uint64_t renderedFrames{0};
    uint64_t droppedFrames{0};
    uint64_t corruptedFrames{0};
};
```

A successful call returns one exact, consistent snapshot for an attached video source. Subscription choices and lost notifications do not affect this query.

| Counter | Meaning |
|---|---|
| `renderedFrames` | Distinct frames eligible for presentation that were presented by the platform video output. |
| `droppedFrames` | Frames eligible for presentation that were discarded under the video-drop rules. |
| `corruptedFrames` | Distinct frames with decoder failure or corruption. Can overlap rendered or dropped frames. |

Total video frames equal `renderedFrames + droppedFrames`. Keep corrupted frames as a separate classification rather than adding them to that total.

Counters increase over the source lifetime. Seek, source flush, pause/play, rate changes, underflow and end of stream preserve them. Repeated display refreshes, frames used only internally by the codec and prepared frames never displayed are excluded. `renderFrame()` counts a displayed paused frame once.

The query returns `false`:

- Before successful `allSourcesAttached()`.
- For an unattached source or a non-video source.
- In `FAILURE`, or after stop has destroyed the video output.
- When a capture is attached.
- When all three counters are unavailable, including older middleware.

The existing `getStats(sourceId, renderedFrames, droppedFrames)` remains supported.

### 6.6 Capture restrictions and retained callbacks

Capture pipelines report observations available before video reaches the application: decode errors, audio drops, audio gaps and underflow. Drops or repeats introduced by application composition are measured by the application.

`getFrameCounters()` returns `false` for capture pipelines because final video presentation takes place in the application.

`setReportDecodeErrors()` is deprecated but retains existing behavior. Selecting decode-error metrics is the new application path. The legacy toggle alone does not enable `notifyMetric()`.

`notifyBufferUnderflow()` remains supported. Selecting `BUFFER_UNDERFLOW` can therefore deliver both forms of notification. `notifyQos()`, `notifyFirstFrameReceived()` and `notifyPlaybackError()` remain independent of the subscription.

### 6.7 Associate measurements with a playback

`IMediaPipeline` adds:

```cpp
virtual bool setPlaybackSessionId(const std::string &playbackSessionId);
```

The application supplies a UUID identifying one logical playback, using the same value with other middleware involved in that playback. A fresh playback, including replay of the same asset, normally uses a new UUID.

The identifier applies to the whole pipeline. It is separate from source identifiers, key-session identifiers, the asset URL and the persistent-license identifier.

Use standard hyphenated UUID text: `8-4-4-4-12` hexadecimal digits. The text is preserved as supplied, with no restriction to a particular UUID generation version. Empty or malformed input is rejected.

Assign initially before `allSourcesAttached()`, including before `load()`. `true` means the identifier is installed. `false` preserves the previous value and covers invalid input or timing, `FAILURE`, communication failure and unsupported middleware.

With no assigned identifier, playback and local metrics remain functional and unassociated. The middleware generates no replacement UUID.

To reuse the same attached sources for a compatible new asset:

1. Pause and wait for `PAUSED`.
2. Either flush every source carrying old-asset media and wait for every `notifySourceFlushed()`, or call `setPosition()` and wait for `SEEK_DONE`.
3. Set the new playback identifier and check success.
4. Configure timestamps and position as needed, supply new media and resume.

Hold renewed data-request responses until assignment succeeds. `SEEK_DONE` confirms the flushing seek call; preparation of new media follows, so waiting for a subsequent `PAUSED` can itself require new media.

Replacement is allowed only after that complete reset and before new media is supplied or presentation resumes. A partial source flush, track switch alone, pending or failed reset, active playback or `STOPPED` is an invalid replacement boundary.

This reuse retains the compatible source set and uses no second `allSourcesAttached()`, `stop()` or `load()`. Ordinary seeks and flushes within the same playback retain the identifier.

Assignment changes no playback state, counters or subscriptions. Queued and late observations from the previous playback keep their original association. The API adds no identifier field to `MetricEvent`.

## 7. Persistent licenses and key status

### 7.1 Acquire and save a persistent license

1. Create `IMediaKeys` for the required key system.
2. Create a session with `KeySessionType::PERSISTENT_LICENCE`.
3. Call `generateRequest()` with the content's initialization data.
4. Send the `onLicenseRequest()` message to the license service and pass its response to `updateSession()`.
5. After a successful update, call `getCdmKeySessionId()` and save the returned identifier.
6. Call `closeKeySession()` when the live session is no longer needed. Stored license data remains available.

The saved identifier is opaque: store and return it unchanged. The integer `keySessionId` identifies a live handle; saving that integer does not allow restoration after a restart.

### 7.2 Restore using the saved identifier

`IMediaKeys` retains the existing operation and adds an overload:

```cpp
virtual MediaKeyErrorStatus loadSession(int32_t keySessionId) = 0;

virtual MediaKeyErrorStatus loadSession(
    int32_t keySessionId, const std::string &persistentSessionId);
```

To restore:

1. Create `IMediaKeys` for the same key system.
2. Create a fresh `PERSISTENT_LICENCE` session and obtain its new `keySessionId`.
3. Call the added overload with that handle and the saved persistent identifier.
4. On `OK`, use the restored session without a new license acquisition.

| `MediaKeyErrorStatus` | Application meaning |
|---|---|
| `OK` | Stored data was found and restored. |
| `SESSION_NOT_FOUND` | No stored session exists for the supplied identifier. Added result value. |
| `NOT_SUPPORTED` | Restore is unsupported by the middleware, platform or key system. |
| `INVALID_STATE` | The handle is not a fresh loadable persistent session, or that persistent identity is already open. |
| `BAD_SESSION_ID` | The target live handle does not exist. |
| `FAIL` | Restore failed because of storage, communication, key-system or internal failure. |

A load attempt initializes the handle even when no stored data is found. After `SESSION_NOT_FOUND`, create a new handle before requesting a new license or trying another saved identifier.

The persistent identifier is supplied to `loadSession()`. Content initialization data continues to belong to `generateRequest()`.

### 7.3 Remove a persistent license completely

`IMediaKeysClient` adds:

```cpp
virtual void onLicenseRelease(int32_t keySessionId,
                              const std::vector<unsigned char> &message,
                              const std::string &url);
```

Applications using persistent-license removal must handle this callback.

1. Call `removeKeySession()` on an open persistent session.
2. Handle the `KeyStatus::RELEASED` update and the release message delivered through `onLicenseRelease()`.
3. Send the release message to the indicated license-service URL for the key system.
4. Pass the service's acknowledgement to `updateSession()`.
5. After that update succeeds, delete the application's saved persistent identifier.

Successful removal acceptance alone does not prove stored data has been deleted. The acknowledgement update completes removal. A later restore then returns `SESSION_NOT_FOUND`.

`closeKeySession()` closes the live session and preserves stored data. Use the removal workflow when the stored license must be deleted.

### 7.4 Added key-status values

`onKeyStatusesChanged()` can report two additional `KeyStatus` values, appended after the existing values:

| Added status | Meaning |
|---|---|
| `OUTPUT_DOWNSCALED` | Output has been reduced in resolution under the content-protection policy. |
| `USABLE_IN_FUTURE` | The key is not yet usable; the key system can report future usability. |

A downscaled-output condition is now reported as `OUTPUT_DOWNSCALED` instead of being folded into `INTERNAL_ERROR`. Adding `USABLE_IN_FUTURE` provides the status representation; actual reporting depends on key-system support.

## 8. Batch decryption

`IMediaPipeline` adds:

```cpp
virtual bool setBatchDecryption(bool batchDecryption) = 0;
```

Batch decryption groups encrypted media buffers for decryption. It can reduce CPU work at the cost of added latency before decryption begins.

- Default: `false` on pipeline creation.
- Call after successful `load()` and before `allSourcesAttached()`.
- `true` enables batch mode; `false` selects individual-buffer decryption.
- The call returns `true` after the setting is effective.
- Invalid call timing, communication failure or unsupported middleware returns `false`.

The batch corresponds to the group of media segments supplied by the client. This control is suited to playback that can tolerate the added latency, such as video on demand. Keep batch mode disabled for low-latency playback that needs immediate decryption.

## 9. Audio presentation delay

`IMediaPipeline` adds:

```cpp
virtual bool setAudioDelay(int64_t delayNs) = 0;
virtual bool getAudioDelay(int64_t &delayNs) const = 0;
```

Applications can use audio presentation delay to help compensate for delayed display when using video textures. This pipeline-level control changes when audio is presented relative to its original presentation timestamp.

| `delayNs` | Effect |
|---|---|
| Positive | Present audio later. |
| Negative | Present audio earlier. |
| Zero | Remove the timing adjustment. |

Use nanoseconds. For example, `50'000'000` requests audio 50 milliseconds later. The adjustment preserves the decoded audio timestamps.

`setAudioDelay()` is valid in `PAUSED` or `PLAYING`. It returns `true` after the setting is effective. Other states, communication failure or unsupported middleware return `false`.

`getAudioDelay()` writes the current delay into its output parameter on success. It returns `false` when audio-delay reporting is unsupported.

When using video textures, measure any extra delay introduced by the application's graphics and display path. Use this control to adjust audio timing to match that path.

## 10. Application migration checklist

| Application need | Action |
|---|---|
| Continue existing playback | Retain existing playback, source, data-delivery and callback usage. |
| Add video texturing for applications that need it | Create a typed capture, attach before source setup, and check setup success. Retain frames through graphics completion. |
| Measure playback quality | Implement metric and missed-event callbacks, then select the required metric types. |
| Report total video frames | Use `getFrameCounters()` where supported; total is rendered plus dropped. Keep corruption separate. |
| Run against older middleware | Check operation results and creation pointers. Keep `getStats()` as the existing two-counter path when the three-counter query is unavailable. |
| Associate a new playback | Assign its UUID before source setup, or at the complete paused reset boundary in §6.7. |
| Save a license for later use | Save the persistent identifier after successful acquisition. Restore it into a fresh live handle. |
| Delete a persistent license | Handle the release callback and complete the service acknowledgement before deleting the saved identifier. |
| Reduce decryption CPU work | Enable batch mode before source setup when its latency is acceptable. |
| Compensate for delayed display when using video textures | Set audio delay in `PAUSED` or `PLAYING` and check success. |

New capture, metric and timing operations can be unavailable on older middleware. Saved-identifier restore reports `NOT_SUPPORTED` when unavailable. Applications must handle these results rather than assuming feature support from successful pipeline creation.

## Appendix A. Revision history

| Document version | Date | Summary |
|---|---|---|
| 0.1 | 2026-11-25 | First draft of this document, covering the Firebolt Media 8 API and Firebolt Media 9 additions and changes. |

### What changed in version 0.1

- Summarized the retained client API by application task.
- Defined capture, media metrics, playback identifiers, persistent-license changes, key statuses, batch decryption and audio presentation delay.
- Defined application ordering, completion handling, frame lifetime and migration steps.
