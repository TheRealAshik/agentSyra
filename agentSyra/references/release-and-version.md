# Release and Version Reporting

## Purpose

Use this reference when a user asks which Syra skill version is running, whether it is current, what changed, or what the official changelog says. This procedure reports version status; it does not publish or install releases.

## Source of the running version

- Read `metadata.version` from the YAML frontmatter of the exact `agentSyra/SKILL.md` revision loaded for this conversation. The repository currently declares `0.1.0-dev` as a development identifier for this version-reporting implementation; it is not an official release claim.
- If the metadata field is missing, malformed, or the loaded revision cannot be identified, report the running version as **unknown**. Do not infer a version from a branch name, current date, commit message, file modification time, cached memory, or the latest published release.
- Report the source of the loaded skill when known (attached package, raw `main`, release tag, or another verified ref). A locally attached ZIP can lag behind the repository; do not claim its contents match GitHub unless they were compared.

## Official release lookup

Use the official GitHub Releases API as the primary source:

`https://api.github.com/repos/TheRealAshik/agentSyra/releases?per_page=100`

Fetch the live response with an available internet-capable tool. Read the actual structured response; a search snippet, URL, repository listing, or cached recollection is not release evidence. For each release, use the official `tag_name`, `name`, `published_at`, `html_url`, `body`, `draft`, and `prerelease` fields when present.

- Ignore draft releases for public-version reporting.
- Separate stable releases (`prerelease: false`) from prereleases (`prerelease: true`). If asked for the latest release generally, identify whether it is a prerelease; for the latest stable version, choose only a published non-draft stable release.
- Normalize a leading `v` on a tag only for version comparison (for example, `v1.2.3` and `1.2.3`). Do not rewrite or misquote the original tag in the displayed result.
- Treat development/prerelease suffixes such as `-dev`, `-alpha`, `-beta`, and `-rc` as prerelease identifiers. Never claim a development identifier is a stable release just because its numeric portion resembles a tag.
- Claim an exact release match only when the loaded metadata version matches a verified release tag under this normalization. A matching release's official title, publication date, URL, and body are the source for its changelog summary.

## How to answer

When asked **“What version are you running?”**, provide:

1. **Loaded skill version** — exact `metadata.version` value, or `unknown`.
2. **Release status** — exact match to an official published release, development/unreleased version, or unverified because data could not be fetched.
3. **Latest stable release** — version/tag and publication date when a verified stable release exists; otherwise say no published stable release was found in the fetched results.
4. **What changed** — a concise summary from the matching release notes. If the loaded version has no matching release, summarize the latest stable release separately and clearly label it as newer/older than the loaded version only when a valid version comparison supports that conclusion.
5. **Official source links** — link to the matching release and/or latest stable release page.

When asked for **the changelog**, summarize the release body's relevant changes rather than inventing missing sections. Preserve any breaking changes, migration notes, security notes, or known limitations that affect the user. Keep the summary concise, and link to the complete official notes.

## No release or failed network cases

- If the API returns an empty release list, state that no published GitHub Releases were found in that response. Do not manufacture a “first release,” release date, tag, or changelog.
- If a development version has no matching release, say it is an unreleased/development revision and that there are no official notes for that exact version. You may separately summarize the latest published stable release if one exists.
- If the request fails, the API is rate-limited, the response is invalid, or required fields are missing, state which part could not be verified. Do not silently use old release data from memory or claim the current version is latest.
- If versions cannot be parsed reliably, show the literal strings and avoid asserting that one is newer or older. Never use lexicographic string comparison as semantic version comparison.
- If more than 100 releases exist and pagination is supported, fetch additional pages only when needed to answer the question. Do not claim the result is exhaustive if pagination was not checked and completeness matters.

## Boundaries

- Reading release metadata and notes is read-only.
- Do not create, delete, or move tags; draft or publish releases; alter version metadata; merge PRs; or deploy as part of answering a version question.
- Keep `SYSTEM_INSTRUCTIONS.md` and `SYSTEM_INSTRUCTION_ONLINE.md` aligned on this behavior. The only intended operating-rule difference between those files is their startup source.
