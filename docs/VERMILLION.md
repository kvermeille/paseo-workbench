# Vermillion Workbench fork

Upstream: https://github.com/julien-bloch/paseo-workbench
Fork: https://github.com/kvermeille/paseo-workbench
Starting commit: `3bdefcf4ea477e47631d92134726949b65df35c0` (0.1.0-alpha.30).

## Licensing status

The upstream checkout has no LICENSE file or declared package license, and GitHub reports no detected license. Licensing clarification is pending. No license is added by this fork. Before adopting this for client operations or distributing a customized release, obtain the author's clarification. Suggested question, not sent: "We would like to maintain a customized Workbench fork for our agency's client coordination. Could you add a license or clarify permission to modify, use and distribute it?"

## Installation fix

Paseo 0.14 validates type-only dependency imports. The registry checkout lacks node_modules, so server imports of `@getpaseo/client` fail before activation. The manifest now explicitly prepares the checkout with `npm ci --include=dev --ignore-scripts --no-audit --no-fund`. Development dependencies are necessary because the SDK type declarations live there. Lifecycle scripts are disabled. Dependencies remain pinned by the upstream lockfile.

Keep the runtime id `workbench` so existing settings and data retain their names. This fork replaces that installation if installed with the default id. Use a disposable daemon for validation before replacing an existing installation.

## Client pilot

1. Install the tested commit of this fork on the intended daemon after licensing is clarified. Select a default model from that host's provider catalog.
2. In Workbench Settings, set the specialists folder to this checkout's absolute `specialists` directory. This adds Client coordinator through Workbench's existing specialist loader.
3. Launch Client coordinator across only the selected client's projects. Provide the client's name, task tracker/project, approved repositories, priorities and verification requirements. Do not choose every project.
4. First ask it to recommend one ready task and its acceptance criteria. Approve that task before it delegates implementation.
5. Check the worker's diff, verification evidence, tracker updates and approval boundaries. Review one completed task before adding another client or schedules.

One coordinator per client is an organizational convention, not enforced access isolation. Configure separate daemon accounts or hosts where clients need an enforced boundary.

## Updating

Keep `upstream` pointed at the original repository and `origin` at the fork. Fetch upstream changes, inspect the diff and lockfile changes, and integrate them on a branch. Run `npm test`, then install the exact candidate on a disposable Paseo daemon and inspect status and logs. Only promote a tested commit. Do not automatically update the working installation from upstream main.

The first stabilization branch deliberately retains upstream SDK dependency versions. Runtime compatibility claims must distinguish successful installation from full client-task acceptance testing. No production coordinator or schedule is created by this change.

## Validation: 2026-10-30

Candidate `158997985d72186d3b2f5315a08db430c065cc42`, Paseo desktop/daemon 0.14.0, Node 24.19.0:

- All 43 upstream unit tests passed; TypeScript typechecking passed.
- The client coordinator Markdown parsed through the existing loader with coordinator-only enabled and read-only disabled.
- Directory installation compiled and reported `running` on a disposable daemon.
- Removed the directory installation, then installed the exact candidate through a Git source. The fresh managed checkout ran the manifest preparation, compiled and reported `running`, resolving the original missing type dependency failure.
- Test daemon used `/tmp/vermillion-paseo-test-home`, localhost port 16868, relay disabled. It was stopped after validation. No production daemon settings, coordinators, schedules or client tasks were changed.

Full client-task behavior, UI interaction and tracker integration remain manual pilot checks. This validation establishes installation and compilation, not production readiness. The test command emitted warnings from an older npm found by nested npm scripts; the preparation step and checks completed successfully.
