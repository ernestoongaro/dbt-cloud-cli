# dbt-cloud-cli

`dbt-cloud-cli` is a command line interface for [dbt Cloud API](https://docs.getdbt.com/dbt-cloud/api-v2). It abstracts the REST API calls in an easy-to-use interface that can be incorporated into automated and manual (ad-hoc) workloads. Here are some example use cases for `dbt-cloud-cli`:

1. Triggering dbt Cloud jobs in CI/CD: You can use [dbt-cloud job run](#dbt-cloud-job-run) in a CI/CD workflow (e.g., Github Actions) to trigger a dbt Cloud job that runs and tests the changes in a commit branch
2. Setting up dbt Cloud jobs: You can use [dbt-cloud job create](#dbt-cloud-job-create) to create standardized jobs between dbt Cloud projects.

## Installation

`dbt-cloud-cli` has been tested on Python 3.8 but it should work on Python>=3.6.

Installation from PyPI:

    pip install dbt-cloud-cli

Installation from GitHub:

    pip install git+https://github.com/data-mie/dbt-cloud-cli.git

## Environment variables

The following environment variables are used as argument defaults:

* `DBT_CLOUD_API_TOKEN` (`--api-token`)
* `DBT_CLOUD_ACCOUNT_ID` (`--account-id`)
* `DBT_CLOUD_JOB_ID` (`--job-id`)
* `DBT_CLOUD_HOST` (`--dbt-cloud-host`)

# Commands

* [dbt-cloud job run](#dbt-cloud-job-run)
* [dbt-cloud job get](#dbt-cloud-job-get)
* [dbt-cloud job list](#dbt-cloud-job-list)
* [dbt-cloud job create](#dbt-cloud-job-create)
* [dbt-cloud job delete](#dbt-cloud-job-delete)
* [dbt-cloud job delete-all](#dbt-cloud-job-delete-all)
* [dbt-cloud job export](#dbt-cloud-job-export)
* [dbt-cloud job import](#dbt-cloud-job-import)
* [dbt-cloud run get](#dbt-cloud-run-get)
* [dbt-cloud run list](#dbt-cloud-run-list)
* [dbt-cloud run list-artifacts](#dbt-cloud-run-list-artifacts)
* [dbt-cloud run get-artifact](#dbt-cloud-run-get-artifact)
* [dbt-cloud metadata query](#dbt-cloud-metadata-query)

## dbt-cloud job run
This command triggers a dbt Cloud job run and returns a status JSON response. For more information on the API endpoint arguments and response, run `dbt-cloud job run --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/triggerRun).

### Usage

```bash
>> dbt-cloud job run --job-id 43167 --cause "My first run!" --wait
Job 43167 run 34929305: QUEUED ...
Job 43167 run 34929305: QUEUED ...
Job 43167 run 34929305: QUEUED ...
Job 43167 run 34929305: STARTING ...
Job 43167 run 34929305: RUNNING ...
Job 43167 run 34929305: SUCCESS ...
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "id": 34929305,
    "trigger_id": 35644346,
    "account_id": REDACTED,
    "environment_id": 49819,
    "project_id": REDACTED,
    "job_definition_id": 43167,
    "status": 10,
    "dbt_version": "0.21.0",
    "git_branch": "main",
    "git_sha": "981c5cf1ba299e942c6c277c38c8dec9b0738dd0",
    "status_message": null,
    "owner_thread_id": null,
    "executed_by_thread_id": "dbt-run-34929305-dcmbq",
    "deferring_run_id": null,
    "artifacts_saved": true,
    "artifact_s3_path": "prod/runs/34929305/artifacts/target",
    "has_docs_generated": false,
    "has_sources_generated": false,
    "notifications_sent": true,
    "blocked_by": [],
    "scribe_enabled": true,
    "created_at": "2021-11-26 16:48:41.431645+00:00",
    "updated_at": "2021-11-26 16:49:33.078918+00:00",
    "dequeued_at": "2021-11-26 16:49:15.670558+00:00",
    "started_at": "2021-11-26 16:49:20.535987+00:00",
    "finished_at": "2021-11-26 16:49:32.996703+00:00",
    "last_checked_at": null,
    "last_heartbeat_at": null,
    "should_start_at": null,
    "trigger": null,
    "job": null,
    "environment": null,
    "run_steps": [],
    "status_humanized": "Success",
    "in_progress": false,
    "is_complete": true,
    "is_success": true,
    "is_error": false,
    "is_cancelled": false,
    "href": REDACTED,
    "duration": "00:00:51",
    "queued_duration": "00:00:39",
    "run_duration": "00:00:12",
    "duration_humanized": "51 seconds",
    "queued_duration_humanized": "39 seconds",
    "run_duration_humanized": "12 seconds",
    "created_at_humanized": "1 minute ago",
    "finished_at_humanized": "9 seconds ago",
    "job_id": 43167
  }
}
```

## dbt-cloud job get
This command returns the details of a dbt Cloud job. For more information on the API endpoint arguments and response, run `dbt-cloud job get --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/getJobById).

### Usage

```bash
>> dbt-cloud job get --job-id 43167
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "execution": {
      "timeout_seconds": 0
    },
    "generate_docs": false,
    "run_generate_sources": false,
    "id": 43167,
    "account_id": REDACTED,
    "project_id": REDACTED,
    "environment_id": 49819,
    "name": "Do nothing!",
    "dbt_version": null,
    "created_at": "2021-11-18T15:19:03.185668+00:00",
    "updated_at": "2021-11-18T15:19:03.185687+00:00",
    "execute_steps": [
      "dbt run -s not_a_model"
    ],
    "state": 1,
    "deferring_job_definition_id": null,
    "lifecycle_webhooks": false,
    "lifecycle_webhooks_url": null,
    "triggers": {
      "github_webhook": false,
      "git_provider_webhook": false,
      "custom_branch_only": true,
      "schedule": false
    },
    "settings": {
      "threads": 4,
      "target_name": "default"
    },
    "schedule": {
      "cron": "0 * * * *",
      "date": {
        "type": "every_day"
      },
      "time": {
        "type": "every_hour",
        "interval": 1
      }
    },
    "is_deferrable": false,
    "generate_sources": false,
    "cron_humanized": "Every hour",
    "next_run": null,
    "next_run_humanized": null
  }
}
```

## dbt-cloud job list
This command returns a list of jobs in the account. For more information on the API endpoint arguments and response, run `dbt-cloud job list --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/listJobsForAccount).

### Usage

```bash
>> dbt-cloud job list
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": [
    {
      "execution": {
        "timeout_seconds": 0
      },
      "generate_docs": false,
      "run_generate_sources": false,
      "id": 43167,
      "account_id": REDACTED,
      "project_id": REDACTED,
      "environment_id": 49819,
      "name": "Do nothing!",
      "dbt_version": null,
      "created_at": "2021-11-18T15:19:03.185668+00:00",
      "updated_at": "2021-12-25T09:17:12.788186+00:00",
      "execute_steps": [
        "dbt run -s not_a_model"
      ],
      "state": 1,
      "deferring_job_definition_id": null,
      "lifecycle_webhooks": false,
      "lifecycle_webhooks_url": null,
      "triggers": {
        "github_webhook": false,
        "git_provider_webhook": null,
        "custom_branch_only": true,
        "schedule": false
      },
      "settings": {
        "threads": 4,
        "target_name": "default"
      },
      "schedule": {
        "cron": "0 * * * *",
        "date": {
          "type": "every_day"
        },
        "time": {
          "type": "every_hour",
          "interval": 1
        }
      },
      "is_deferrable": false,
      "generate_sources": false,
      "cron_humanized": "Every hour",
      "next_run": null,
      "next_run_humanized": null
    }
  ]
}
```

## dbt-cloud job create

This command creates a job in a dbt Cloud project. For more information on the API endpoint arguments and response, run `dbt-cloud job create --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/createJob).

### Usage
```bash
dbt-cloud job create --project-id REFACTED --environment-id 49819 --name "Create job" --execute-steps "dbt seed" --execute-steps "dbt run"
{
  "status": {
    "code": 201,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "execution": {
      "timeout_seconds": 0
    },
    "generate_docs": false,
    "run_generate_sources": false,
    "id": 48180,
    "account_id": REDACTED,
    "project_id": REDACTED,
    "environment_id": 49819,
    "name": "Create job",
    "dbt_version": null,
    "created_at": "2021-12-22T11:23:26.968076+00:00",
    "updated_at": "2021-12-22T11:23:26.968094+00:00",
    "execute_steps": [
      "dbt seed",
      "dbt run"
    ],
    "state": 1,
    "deferring_job_definition_id": null,
    "lifecycle_webhooks": false,
    "lifecycle_webhooks_url": null,
    "triggers": {
      "github_webhook": false,
      "git_provider_webhook": null,
      "custom_branch_only": false,
      "schedule": false
    },
    "settings": {
      "threads": 1,
      "target_name": "default"
    },
    "schedule": {
      "cron": "0 * * * *",
      "date": {
        "type": "every_day"
      },
      "time": {
        "type": "every_hour",
        "interval": 1
      }
    },
    "is_deferrable": false,
    "generate_sources": false,
    "cron_humanized": "Every hour",
    "next_run": null,
    "next_run_humanized": null
  }
}
```

## dbt-cloud job delete

This command deletes a job in a dbt Cloud project. Note that this command uses an undocumented v3 API endpoint. For more information on the command and its arguments, run `dbt-cloud job delete --help`.

### Usage

```bash
>> dbt-cloud job delete --job-id 48474
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "execution": {
      "timeout_seconds": 0
    },
    "generate_docs": false,
    "run_generate_sources": false,
    "id": 48474,
    "account_id": REDACTED,
    "project_id": REDACTED,
    "environment_id": 49819,
    "name": "Do nothing!",
    "dbt_version": null,
    "created_at": "2021-12-25T10:12:29.114456+00:00",
    "updated_at": "2021-12-25T10:12:29.814383+00:00",
    "execute_steps": [
      "dbt run -s not_a_model"
    ],
    "state": 2,
    "deferring_job_definition_id": null,
    "lifecycle_webhooks": false,
    "lifecycle_webhooks_url": null,
    "triggers": {
      "github_webhook": false,
      "git_provider_webhook": null,
      "custom_branch_only": true,
      "schedule": false
    },
    "settings": {
      "threads": 4,
      "target_name": "default"
    },
    "schedule": {
      "cron": "0 * * * *",
      "date": {
        "type": "every_day"
      },
      "time": {
        "type": "every_hour",
        "interval": 1
      }
    },
    "is_deferrable": false,
    "generate_sources": false,
    "cron_humanized": "Every hour",
    "next_run": null,
    "next_run_humanized": null
  }
}
```

## dbt-cloud job delete-all

This command fetches all jobs on the account, deletes them one-by-one after user confirmation via prompt and prints out the job delete responses. For more information on the command and its arguments, run `dbt-cloud job delete-all --help`.

### Usage

```bash
>> dbt-cloud job delete-all --keep-jobs "[43167, 49663]"
Jobs to delete: [54658, 54659]
Delete job 54658? [y/N]: yes
Job 54658 was deleted.
Delete job 54659? [y/N]: yes
Job 54659 was deleted.
[
  {
    "status": {
      "code": 200,
      "is_success": true,
      "user_message": "Success!",
      "developer_message": ""
    },
    "data": {
      "execution": {
        "timeout_seconds": 0
      },
      "generate_docs": false,
      "run_generate_sources": false,
      "id": 54658,
      "account_id": REDACTED,
      "project_id": REDACTED,
      "environment_id": 49819,
      "name": "Do nothing!",
      "dbt_version": null,
      "created_at": "2022-01-27T09:04:26.080595+00:00",
      "updated_at": "2022-01-27T09:05:10.527583+00:00",
      "execute_steps": [
        "dbt run -s not_a_model"
      ],
      "state": 2,
      "deferring_job_definition_id": null,
      "lifecycle_webhooks": false,
      "lifecycle_webhooks_url": null,
      "triggers": {
        "github_webhook": false,
        "git_provider_webhook": null,
        "custom_branch_only": true,
        "schedule": false
      },
      "settings": {
        "threads": 4,
        "target_name": "default"
      },
      "schedule": {
        "cron": "0 * * * *",
        "date": {
          "type": "every_day"
        },
        "time": {
          "type": "every_hour",
          "interval": 1
        }
      },
      "is_deferrable": false,
      "generate_sources": false,
      "cron_humanized": "Every hour",
      "next_run": null,
      "next_run_humanized": null
    }
  },
  {
    "status": {
      "code": 200,
      "is_success": true,
      "user_message": "Success!",
      "developer_message": ""
    },
    "data": {
      "execution": {
        "timeout_seconds": 0
      },
      "generate_docs": false,
      "run_generate_sources": false,
      "id": 54659,
      "account_id": REDACTED,
      "project_id": REDACTED,
      "environment_id": 49819,
      "name": "Do nothing!",
      "dbt_version": null,
      "created_at": "2022-01-27T09:04:43.231873+00:00",
      "updated_at": "2022-01-27T09:05:19.533456+00:00",
      "execute_steps": [
        "dbt run -s not_a_model"
      ],
      "state": 2,
      "deferring_job_definition_id": null,
      "lifecycle_webhooks": false,
      "lifecycle_webhooks_url": null,
      "triggers": {
        "github_webhook": false,
        "git_provider_webhook": null,
        "custom_branch_only": true,
        "schedule": false
      },
      "settings": {
        "threads": 4,
        "target_name": "default"
      },
      "schedule": {
        "cron": "0 * * * *",
        "date": {
          "type": "every_day"
        },
        "time": {
          "type": "every_hour",
          "interval": 1
        }
      },
      "is_deferrable": false,
      "generate_sources": false,
      "cron_humanized": "Every hour",
      "next_run": null,
      "next_run_humanized": null
    }
  }
]
```

## dbt-cloud job export

This command exports a dbt Cloud job as JSON to a file and can be used in conjunction with [dbt-cloud job import](#dbt-cloud-job-import) to copy jobs between dbt Cloud projects.

### Usage

```bash
>> dbt-cloud job export | tee job.json
{
  "execution": {
    "timeout_seconds": 0
  },
  "generate_docs": false,
  "run_generate_sources": false,
  "account_id": REDACTED,
  "project_id": REDACTED,
  "environment_id": 49819,
  "name": "Do nothing!",
  "dbt_version": null,
  "created_at": "2021-11-18T15:19:03.185668+00:00",
  "updated_at": "2021-12-25T09:17:12.788186+00:00",
  "execute_steps": [
    "dbt run -s not_a_model"
  ],
  "state": 1,
  "deferring_job_definition_id": null,
  "lifecycle_webhooks": false,
  "lifecycle_webhooks_url": null,
  "triggers": {
    "github_webhook": false,
    "git_provider_webhook": null,
    "custom_branch_only": true,
    "schedule": false
  },
  "settings": {
    "threads": 4,
    "target_name": "default"
  },
  "schedule": {
    "cron": "0 * * * *",
    "date": {
      "type": "every_day"
    },
    "time": {
      "type": "every_hour",
      "interval": 1
    }
  },
  "is_deferrable": false,
  "generate_sources": false,
  "cron_humanized": "Every hour",
  "next_run": null,
  "next_run_humanized": null
}
```

## dbt-cloud job import

This command imports a dbt Cloud job from exported JSON. You can use JSON manipulation tools (e.g., [jq](https://stedolan.github.io/jq/)) to modify the job definition before importing it.

### Usage

```bash
>> cat job.json | jq '.environment_id = 49819 | .name = "Imported job"' | dbt-cloud job import
{
  "status": {
    "code": 201,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "execution": {
      "timeout_seconds": 0
    },
    "generate_docs": false,
    "run_generate_sources": false,
    "id": 48475,
    "account_id": REDACTED,
    "project_id": REDACTED,
    "environment_id": 49819,
    "name": "Imported job",
    "dbt_version": null,
    "created_at": "2021-12-25T10:40:13.193129+00:00",
    "updated_at": "2021-12-25T10:40:13.193149+00:00",
    "execute_steps": [
      "dbt run -s not_a_model"
    ],
    "state": 1,
    "deferring_job_definition_id": null,
    "lifecycle_webhooks": false,
    "lifecycle_webhooks_url": null,
    "triggers": {
      "github_webhook": false,
      "git_provider_webhook": null,
      "custom_branch_only": true,
      "schedule": false
    },
    "settings": {
      "threads": 4,
      "target_name": "default"
    },
    "schedule": {
      "cron": "0 * * * *",
      "date": {
        "type": "every_day"
      },
      "time": {
        "type": "every_hour",
        "interval": 1
      }
    },
    "is_deferrable": false,
    "generate_sources": false,
    "cron_humanized": "Every hour",
    "next_run": null,
    "next_run_humanized": null
  }
}
```

## dbt-cloud run get
This command prints a dbt Cloud run status JSON response. For more information on the API endpoint arguments and response, run `dbt-cloud run get --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/getRunById).

### Usage

```bash
>> dbt-cloud run get --run-id 36053848
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": {
    "id": 36053848,
    "trigger_id": 36768889,
    "account_id": REDACTED,
    "environment_id": 49819,
    "project_id": REDACTED,
    "job_definition_id": 43167,
    "status": 10,
    "dbt_version": "0.21.0",
    "git_branch": "main",
    "git_sha": "981c5cf1ba299e942c6c277c38c8dec9b0738dd0",
    "status_message": null,
    "owner_thread_id": null,
    "executed_by_thread_id": "dbt-run-36053848-84vsp",
    "deferring_run_id": null,
    "artifacts_saved": true,
    "artifact_s3_path": "prod/runs/36053848/artifacts/target",
    "has_docs_generated": false,
    "has_sources_generated": false,
    "notifications_sent": true,
    "blocked_by": [],
    "scribe_enabled": true,
    "created_at": "2021-12-07 10:32:24.326116+00:00",
    "updated_at": "2021-12-07 10:34:14.507280+00:00",
    "dequeued_at": "2021-12-07 10:33:54.599925+00:00",
    "started_at": "2021-12-07 10:34:01.982824+00:00",
    "finished_at": "2021-12-07 10:34:14.435474+00:00",
    "last_checked_at": null,
    "last_heartbeat_at": null,
    "should_start_at": null,
    "trigger": null,
    "job": null,
    "environment": null,
    "run_steps": [],
    "status_humanized": "Success",
    "in_progress": false,
    "is_complete": true,
    "is_success": true,
    "is_error": false,
    "is_cancelled": false,
    "href": REDACTED,
    "duration": "00:01:50",
    "queued_duration": "00:01:37",
    "run_duration": "00:00:12",
    "duration_humanized": "1 minute, 50 seconds",
    "queued_duration_humanized": "1 minute, 37 seconds",
    "run_duration_humanized": "12 seconds",
    "created_at_humanized": "2 weeks, 1 day ago",
    "finished_at_humanized": "2 weeks, 1 day ago",
    "job_id": 43167
  }
}
```

## dbt-cloud run list
This command returns a list of runs in the account. For more information on the API endpoint arguments and response, run `dbt-cloud run list --help` and check out the [dbt Cloud API v4 docs](https://docs.getdbt.com/dbt-cloud/api-v4#operation/list-account-runs).

### Usage

```bash
>> dbt-cloud run list --limit 2
{
  "data": [
    {
      "id": "40650768",
      "environment_id": "49819",
      "account_id": REDACTED,
      "project_id": REDACTED,
      "job_id": "49663",
      "trigger": {
        "cause": "scheduled",
        "git_provider": null,
        "pull_request_id": null,
        "git_branch": "main",
        "git_sha": "981c5cf1ba299e942c6c277c38c8dec9b0738dd0"
      },
      "replace": {
        "schema_with": null,
        "target_name_with": null,
        "dbt_version_with": null,
        "generate_docs_with": null,
        "run_steps_with": null,
        "thread_count_with": null,
        "timeout_after_with": null
      },
      "href": "https://cloud.getdbt.com/api/v4/accounts/REDACTED/runs/40650768",
      "status": "Succeeded",
      "status_message": null,
      "dbt_version": "0.21.0",
      "waiting_on": [],
      "triggered_at": 1642377765,
      "created_at": 1642377765,
      "updated_at": 1642378181,
      "dequeued_at": 1642377904,
      "started_at": 1642378141,
      "finished_at": 1642378181,
      "duration": 416,
      "queued_duration": 376,
      "run_duration": 40,
      "artifacts_saved": true,
      "has_docs_generated": true,
      "has_sources_generated": false
    },
    {
      "id": "40538548",
      "environment_id": "49819",
      "account_id": REDACTED,
      "project_id": REDACTED,
      "job_id": "49663",
      "trigger": {
        "cause": "scheduled",
        "git_provider": null,
        "pull_request_id": null,
        "git_branch": "main",
        "git_sha": "981c5cf1ba299e942c6c277c38c8dec9b0738dd0"
      },
      "replace": {
        "schema_with": null,
        "target_name_with": null,
        "dbt_version_with": null,
        "generate_docs_with": null,
        "run_steps_with": null,
        "thread_count_with": null,
        "timeout_after_with": null
      },
      "href": "https://cloud.getdbt.com/api/v4/accounts/REDACTED/runs/40538548",
      "status": "Succeeded",
      "status_message": null,
      "dbt_version": "0.21.0",
      "waiting_on": [],
      "triggered_at": 1642291308,
      "created_at": 1642291308,
      "updated_at": 1642291725,
      "dequeued_at": 1642291455,
      "started_at": 1642291683,
      "finished_at": 1642291725,
      "duration": 417,
      "queued_duration": 375,
      "run_duration": 42,
      "artifacts_saved": true,
      "has_docs_generated": true,
      "has_sources_generated": false
    }
  ]
}
```

## dbt-cloud run list-artifacts
This command fetches a list of artifact files generated for a completed run. For more information on the API endpoint arguments and response, run `dbt-cloud run list-artifacts --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/listArtifactsByRunId).

### Usage

```bash
>> dbt-cloud run list-artifacts --run-id 36053848
{
  "status": {
    "code": 200,
    "is_success": true,
    "user_message": "Success!",
    "developer_message": ""
  },
  "data": [
    "manifest.json",
    "run/jaffle_shop/data/raw_customers.csv",
    "run/jaffle_shop/data/raw_orders.csv",
    "run/jaffle_shop/data/raw_payments.csv",
    "run_results.json"
  ]
}
```

## dbt-cloud run get-artifact
This command fetches an artifact file from a completed run. Once a run has completed, you can use this command to download the manifest.json, run_results.json or catalog.json files from dbt Cloud. These artifacts contain information about the models in your dbt project, timing information around their execution, and a status message indicating the result of the model build.

For more information on the API endpoint arguments and response, run `dbt-cloud run get-artifact --help` and check out the [dbt Cloud API docs](https://docs.getdbt.com/dbt-cloud/api-v2#operation/getArtifactsByRunId).

### Usage

```bash
>> dbt-cloud run get-artifact --run-id 36053848 --path manifest.json > manifest.json
```

## dbt-cloud metadata query
This command queries the dbt Cloud Metadata API using GraphQL. For more information on the Metadata API, see [the docs](https://docs.getdbt.com/docs/dbt-cloud/dbt-cloud-api/metadata/metadata-overview).

### Usage

```bash
>> dbt-cloud metadata query -f query.graphql
{
  "data": {
    "model": {
      "parentsModels": [
        {
          "runId": 39352464,
          "uniqueId": "model.jaffle_shop.stg_orders",
          "executionTime": 0.870538949966431
        },
        {
          "runId": 39352464,
          "uniqueId": "model.jaffle_shop.stg_payments",
          "executionTime": 0.635890483856201
        },
        {
          "runId": 39352464,
          "uniqueId": "model.jaffle_shop.stg_customers",
          "executionTime": 0.697099924087524
        }
      ],
      "parentsSources": []
    }
  }
}
```

```graphql
""" query.graphql """
{
  model(jobId: 49663, uniqueId: "model.jaffle_shop.customers") {
    parentsModels {
      runId
      uniqueId
      executionTime
    }
    parentsSources {
      runId
      uniqueId
      state
    }
  }
}
```

## Acknowledgements

Thanks to [Sean McInture](https://github.com/boxysean) for his initial work on triggering a dbt Cloud job using Python as proposed in [this post on dbt Discourse](https://discourse.getdbt.com/t/triggering-a-dbt-cloud-job-in-your-automated-workflow-with-python/2573). Thank you for sharing your work with the community!
