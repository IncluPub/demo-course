# IncluPub demo course

This repository is a course in the [IncluPub](https://inclu.pub/) format, set up the way courses of the tollwerk Academy are: the source of the course in the top folder, its media in Git LFS, and a pipeline that only includes the [CI/CD component of the IncluPub compiler](https://code.tollwerk.net/inclupub/compiler) in a pinned version. It shows what a course repository looks like and tests the whole tool chain, from the source to the published package.

The content comes from the [example course](https://code.tollwerk.net/inclupub/inclupub/-/tree/main/examples/example-course) of the specification, with identifiers of its own. The example course stays the normative reference and covers every feature of the format; this demo course may diverge from it to show a realistic course.

## Structure

| Path | Content |
| --- | --- |
| `course.yaml` | properties of the course and the order of its modules |
| `course-intro.md`, `course-description.md` | course introduction and course description |
| `course-assessment.md` | assessment at course level |
| `modules/` | one folder per module with `module.yaml`, its introduction, lessons, summary and assessment |
| `glossary/`, `personas/` | glossary entries and personas, one file each |
| `media/` | images, video and audio with captions, transcripts and properties; binary media in Git LFS |
| `certificate.md`, `pronunciation.yaml` | certificate information and pronunciation lexicon |

The [source specification](https://code.tollwerk.net/inclupub/inclupub/-/blob/main/spec/source.md) explains every file.

## Build

The pipeline validates the source and builds the package on every commit; the package is an artifact of the pipeline, not part of the repository. The units are narrated with the default voice of the compiler, which the pipeline loads with its job token; this project is in the job token allowlist of [`inclupub/voice`](https://code.tollwerk.net/inclupub/voice). The job `inclupub:check` checks it with the checker of the specification and EPUBCheck. A tag `v<version>` publishes the package to the package registry of this project.

Locally, with the image of the compiler:

```sh
docker run --rm -v "$PWD:/builds" images.tollwerk.net/inclupub/compiler inclupub validate .
docker run --rm -v "$PWD:/builds" images.tollwerk.net/inclupub/compiler inclupub build . --output demo-course.epub
```

With narration, the voice needs a personal access token with access to `inclupub/voice`:

```sh
docker run --rm -v "$PWD:/builds" -e INCLUPUB_VOICE_TOKEN images.tollwerk.net/inclupub/compiler inclupub build . --output demo-course.epub --narrate
```

The skeleton of this repository was created with `inclupub new`.
