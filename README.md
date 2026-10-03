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
| `back/` | appendices at the end of the course, one file each, in the order of `course.yaml` |
| `glossary/`, `personas/` | glossary entries and personas, one file each |
| `bibliography.yaml` | references for the citations of the course, from which the bibliography is built |
| `media/` | images, video and audio with captions, transcripts and properties; binary media in Git LFS |
| `certificate.md`, `pronunciation.yaml` | certificate information and pronunciation lexicon |
| `changes.md` | version history of the course |

The [source specification](https://code.tollwerk.net/inclupub/inclupub/-/blob/main/spec/source.md) explains every file.

## Build

The pipeline validates the source and builds the package on every commit; the package is an artifact of the pipeline, not part of the repository. The units are narrated with the default voice of the compiler, which the pipeline loads with its job token; this project is in the job token allowlist of [`inclupub/voice`](https://code.tollwerk.net/inclupub/voice). With the base address for media of the group, `INCLUPUB_MEDIA_BASE`, the pipeline also builds the package with external media, whose video and audio files are delivered from the media storage of the tollwerk Academy. The job `inclupub:check` checks both packages with the validation of the IncluPub library and EPUBCheck. A tag `v<version>` first transfers the external files that are not in the media storage yet and then publishes both packages to the package registry of this project. The access to the media storage is in protected CI/CD variables of the group, so the tags `v*` of this project are protected.

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

## Mirror at GitHub

This repository is mirrored to [inclupub/demo-course at GitHub](https://github.com/inclupub/demo-course), where the workflow for GitHub Actions in `.github/workflows/inclupub.yml` builds, narrates, checks and publishes the course as well; GitLab ignores that file. Every push builds on both sides, and a tag `v*` publishes the package in the package registry here and as a release at GitHub. The workflow was written by `inclupub github-workflow` of compiler 0.11.0, with narration switched on; an update of the compiler writes it again.

The mirror is a push mirror of this project (**Settings**, **Repository**, **Mirroring repositories**) with a fine-grained token at GitHub that may write the contents and the workflows of that one repository. How the organization, the environment `inclupub`, the secrets and the token are set up is described in the [guide for GitHub of the compiler](https://code.tollwerk.net/inclupub/compiler/-/blob/main/docs/github.md), sections 2 to 5 and 9. Renew the token before it expires; an expired token stops the mirror silently.

## License

The demo course is licensed under CC BY 4.0, except two photos that keep their own free licenses. See [LICENSE.md](LICENSE.md).
