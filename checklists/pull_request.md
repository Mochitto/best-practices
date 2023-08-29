## Before creating the merge request:
- [ ] Rebase on `development` branch to make sure you are up-to-date with all the changes made by other people
- [ ] Run tests to check if something has to be fixed
- [ ] If integration tests are not present, run the project and manually check that everything works as expected
- [ ] Create a new commit with the message `Docs: bumped version to v<new-version>` where you modify these files:
    - [ ] `package.json`: `version: <new-version>`
    - [ ] `CHANGELOG.md`: from `## [unreleased]` to `## [<new-version>]`
- [ ] Open a merge request on GitHub and assign a colleague to its review

## Ater the merge request has been accepted
- [ ] Tag the merge commit with the `<new-version>` with `git tag v<new-version>`
- [ ] Push the tag `git push origin v<new-version>`

## Careful
- Versions DO NOT have `v` in front of them in the `CHANGELOG` file and `package.json`.
- Versions DO have the `v` in front of them in tags
