# schedule-outage

[![Test](https://github.com/wpbonelli/schedule-outage/actions/workflows/test.yml/badge.svg)](https://github.com/wpbonelli/schedule-outage/actions/workflows/test.yml)
[![Project Status: WIP – Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](https://www.repostatus.org/badges/latest/wip.svg)](https://www.repostatus.org/#wip)
[![GitHub tag](https://img.shields.io/github/tag/wpbonelli/schedule-outage.svg)](https://github.com/wpbonelli/schedule-outage/tags/latest)

Disable an action or workflow randomly or periodically.

Useful e.g. to signal deprecation, or (if your action calls a service you control) for maintenance windows.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Usage](#usage)
- [Inputs](#inputs)
- [Notes](#notes)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Usage

```yml
# warn that an action will be archived and suggest migration
- uses: wpbonelli/schedule-outage@v1
  with:
    probability: 0.1
    message: 'this/action retires soon, use something/else@v1'

# disable actions clients during a weekly maintenance window
# note: no guarantees that your service will not be invoked!
# somebody could just fork your action and remove this step.
- uses: wpbonelli/schedule-outage@v1
  with:
    schedule: '0 0 * * 0'
    duration: '1h'
    message: 'Down for maintenance, back in 1 hour'
```

## Inputs

| Name | Description | Required | Default |
| --- | --- | --- | --- |
| `schedule` | Cron expression for periodic outages | No | - |
| `duration` | Duration of scheduled outage (hours) | No | 1 (if `schedule` provided) |
| `probability` | Probability of outage | No | - |
| `message` | Message to display | No | 'Action or workflow disabled' |

## Notes

* if `duration` is provided without `schedule`, it will be ignored

* `schedule` and `probability` can be used together &mdash; if both are provided, an outage occurs probabilistically at the scheduled times

* the action installs [`croniter`](https://github.com/kiorky/croniter) if `schedule` is provided &mdash; if pip install fails, it aborts without failing the workflow