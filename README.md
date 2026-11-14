# audio-delivery-checker

A small, beginner-friendly command-line tool for checking WAV files before delivery. It reads standard PCM WAV files and reports their sample rate, bit depth, channels, duration, and peak level. It also flags clipping and common delivery-format mismatches.

It is deliberately lightweight: the checker uses only Python's standard library.

## What it checks

- Whether a WAV file can be opened
- Sample rate, bit depth, and channel count
- Duration
- Peak level (for PCM WAV files) and clipping at full scale
- Optional target sample rate, bit depth, and channels

## Requirements

- Python 3.9 or newer

## Install

Clone the repository, then from its folder run:

```bash
python -m pip install -e .
```

Or run the source directly without installing it:

```bash
python -m audio_delivery_checker path/to/audio.wav
```

## Examples

Check one file:

```bash
audio-delivery-checker mix.wav
```

Check every WAV file in a folder (including subfolders):

```bash
audio-delivery-checker ./exports
```

Check against a delivery specification and save a JSON report:

```bash
audio-delivery-checker ./exports \
  --sample-rate 48000 \
  --bit-depth 24 \
  --channels 2 \
  --json-report delivery-report.json
```

Example terminal output:

```text
audio-delivery-checker: 1 file checked

mix.wav
  PASS
  Format: 48,000 Hz, 24-bit, 2 channels, 03:42.130
  Peak: -1.02 dBFS

Summary: 1 passed, 0 warnings, 0 failed
```

## Notes and limitations

- The peak calculation supports uncompressed PCM WAV data (8-, 16-, 24-, and 32-bit integer samples).
- A full-scale sample is reported as clipping. For floating-point WAV and uncommon WAV encodings, the tool still reads any metadata Python can open but may say that peak analysis is unavailable.
- This tool is a delivery pre-flight check, not a replacement for listening or for a mastering engineer's review.

## Development

Run the tests:

```bash
python -m unittest discover -s tests -v
```

## License

MIT. See [LICENSE](LICENSE).
