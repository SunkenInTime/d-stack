# Attachments

## Upload

Dara's account has read-only access to `pingdotgg/t3code`, so `gh --attach` fails there. Upload each file with the `upload-to-r2` skill and embed the returned URL in the PR body with `--body-file`:

```powershell
python "$env:USERPROFILE\.codex\skills\upload-to-r2\scripts\upload.py" --prefix "<pr-topic>" "E:\path\to\before.png"
```

GitHub renders images and GIFs from these URLs inline. It does not inline an external MP4, so use a GIF for anything that has to play in the PR body, and link the MP4 only as a higher-quality extra.

On repos where Dara has write access, `gh pr edit PR_NUMBER --attach ./before.png` also works.

## Capture

- **Client:** for T3 web, drive a dev build with Playwright and Edge (`channel: "msedge"`). Use a persistent profile and a 1280×800 viewport, plus `deviceScaleFactor: 2` for stills. Keep scripts and profiles in the worktree's gitignored `.t3/`.
- **Before/after:** use the same build and swap only the fixed file back to the base version. `node --watch` restarts the server. Start a fresh thread for each run.
- **Small changes:** crop the changed element from both stills and stack them with labels:

  ```sh
  ffmpeg -i before.png -vf "crop=W:H:X:Y,pad=W:H+60:0:60:white,drawtext=fontfile=font.ttf:text='Before ...':x=12:y=14:fontsize=30" b.png
  ffmpeg -i b.png -i a.png -filter_complex vstack=inputs=2 compare.png
  ```

  Keep colons out of `drawtext` text and copy the font next to the files; Windows drive-letter paths break the filter.
- **GIF:** record with Playwright's `recordVideo`, trim to the moment, and convert with a palette:

  ```sh
  ffmpeg -ss START -i run.webm -t 14 -vf "fps=10,scale=1000:-1:flags=lanczos,split[a][b];[a]palettegen=max_colors=128[p];[b][p]paletteuse=dither=bayer:bayer_scale=4" run.gif
  ```

  Before uploading, pull two frames to confirm the GIF shows the change, not just the setup:

  ```sh
  ffmpeg -i run.gif -vf "select='eq(n\,20)+eq(n\,130)',scale=640:-1,tile=2x1" -frames:v 1 check.png
  ```

## Verify

Open the PR after editing and confirm every image renders. Never commit PR-only media to the repository.
