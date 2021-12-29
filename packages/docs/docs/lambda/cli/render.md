---
id: render
sidebar_label: render
title: "npx remotion lambda render"
slug: /lambda/cli/render
---

Using the `npx remotion lambda render` command, you can render a video in the cloud.

The structure of a command is as follows:

```
npx remotion lambda render <serve-url> <composition-id> [<output-location>]
```

- The serve URL is obtained by deploying a project to Remotion using the [`sites create`](/docs/lambda/cli/sites#create) command or calling [`deploySite()`](/docs/lambda/deploysite).
- The composition ID is the [`id` of your `<Composition/>`](/docs/the-fundamentals#defining-compositions).
- The `output-location` parameter is optional. If you don't specify it, the video is stored in your S3 bucket. If you specify a location, it gets downloaded to your device in an additional step.

## Example commands

Rendering a video:

```
npx remotion lambda render https://remotionlambda-abcdef.s3.eu-central-1.amazonaws.com/sites/testbed/index.html my-comp
```

Rendering a video and saving it to `out/video.mp4`:

```
npx remotion lambda render https://remotionlambda-abcdef.s3.eu-central-1.amazonaws.com/sites/testbed/index.html my-comp out/video.mp4
```

Using the shorthand serve URL:

```
npx remotion lambda render testbed my-comp
```

Passing in input props:

```
npx remotion lambda render --props='{"hi": "there"}' testbed my-comp
```

Printing debug information including a CloudWatch link:

```
npx remotion lambda render --log=verbose testbed my-comp
```

Keeping the output video private:

```
npx remotion lambda render --privacy=private testbed my-comp
```

Rendering only the audio:

```
npx remotion lambda render --codec=mp3 testbed my-comp
```

## Flags

### `--region`

The [AWS region](/docs/lambda/region-selection) to select. Both project and function should be in this region.

### `--props`

[React Props to pass to the root component of your video.](/docs/parametrized-rendering#passing-input-props-in-the-cli) Must be a serialized JSON string (`--props='{"hello": "world"}'`) or a path to a JSON file (`./path/to/props.json`).

### `--log`

Log level to be used inside the Lambda function. Also, if you set it to `verbose`, a link to CloudWatch will be printed where you can inspect logs.

### `--privacy`

Defines if the output media is accessible for everyone or not. Either `public` or `private`, default `public`.

### `--max-retries`

How many times a single chunk is being retried if it fails to render. Default `1`.

### `--disable-chunk-optimization`

Disables [chunk optimization](/docs/lambda/chunk-optimization). By default this flag is false, meaning chunk optimization is enabled.

### `--frames-per-lambda`

How many frames should be rendered in a single Lambda function. Increase it to require less Lambda functions to render the video, decrease it to make the render faster. Default `20`.

### `--quality`

[Value between 0 and 100 for JPEG rendering quality](/docs/config#setquality). Doesn't work when PNG frames are rendered.

### `--codec`

[`h264` or `h265` or `png` or `vp8` or `vp9` or `mp3` or `aac` or `wav` or `prores` or `h264-mkv`](/docs/config#setcodec). If you don't supply `--codec`, it will use `h264-mkv`.

### `--prores-profile`

[Set the ProRes profile](/docs/config#setproresprofile). This option is only valid if the [`codec`](#--codec) has been set to `prores`. Possible values: `4444-xq`, `4444`, `hq`, `standard`, `light`, `proxy`. See [here](https://video.stackexchange.com/a/14715) for explanation of possible values. Default: `hq`.

### `--crf`

[To set Constant Rate Factor (CRF) of the output](/docs/config#setcrf). Minimum 0. Use this rate control mode if you want to keep the best quality and care less about the file size.

### `--pixel-format`

[Set a custom pixel format. See here for available values.](/docs/config#setpixelformat)

### `--image-format`

[`jpeg` or `png` - JPEG is faster, but doesn't support transparency.](/docs/config#setimageformat) The default image format is `jpeg` since v1.1.

### `--env-file`

Specify a location for a dotenv file. Default `.env`.

### `--frames`

[Render a subset of a video](/docs/config#setframerange). Example: `--frames=0-9` to select the first 10 frames. To render a still, use the [`still`](/docs/lambda/cli/still) command.