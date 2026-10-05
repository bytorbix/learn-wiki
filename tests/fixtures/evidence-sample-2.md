# Evidence — demo-service — 2026-10-02

## timestamp-alignment
- 2026-10-02 | demo-service | strong | understanding | designed | Explained why the tolerance window size trades missed matches for wrong matches.
- 2026-10-02 | demo-service | strong | understanding | chose | Chose a 50 ms tolerance over 200 ms, reasoning from the frame rate.
- 2026-10-02 | demo-service | strong | practical | designed | Handled frames with no telemetry within the tolerance window by marking them unmatched instead of dropping them.

## hls
- 2026-10-02 | demo-service | gap | understanding | followed | Couldn't explain why HLS adds several seconds of latency.

## interpolation
- 2026-10-02 | demo-service | gap | practical | followed | Interpolated across a gap larger than the tolerance window without noticing.

## new: clock drift
- 2026-10-02 | demo-service | gap | understanding | followed | Didn't know that two device clocks drift apart over hours.
