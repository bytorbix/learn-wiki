# Evidence — sample-app — 2026-09-28

## new: timestamp-alignment
- 2026-09-28 | sample-app | strong | understanding | designed | Explained why exact timestamp match fails with jittered telemetry; chose nearest-frame matching.
- 2026-09-28 | sample-app | gap | practical | followed | Unsure how to handle frames with no telemetry within the tolerance window.

## new: hls streaming
- 2026-09-28 | sample-app | exposure | understanding | followed | Claude explained how HLS playlists and segments differ from an RTSP stream.

## new: interpolation
- 2026-09-28 | sample-app | strong | practical | implemented | Wrote the nearest-or-exact frame interpolator and fixed an off-by-one in the search.
- 2026-09-28 | sample-app | strong | understanding | followed | Agreed with Claude's explanation of linear interpolation.

## new: networking
- 2026-09-28 | sample-app | strong | understanding | designed | Explained why HLS tolerates network jitter better than raw RTSP for this pipeline.

## new: jsonl output
- 2026-09-28 | sample-app | strong | practical
