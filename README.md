# aws-monitoring-iam
This Terraform module creates CloudWatch Log Metric Filters and associated Alarms for monitoring AWS Identity and Access Management (IAM) based on specified event names. It helps ensure that critical changes to IAM resources are monitored effectively, and alerts are sent to a pre-existing SNS topic.

## Features
- Creates CloudWatch Log Metric Filters for specified IAM events.
- Creates CloudWatch Alarms that trigger based on metrics from the filters.
- Flexible configuration for events to monitor and alarm settings.

## Requirements
- Terraform 1.0 or later
- AWS Provider

## Inputs
| Variable                                     | Description                                                                                          | Type          | Default                                                   |
|----------------------------------------------|------------------------------------------------------------------------------------------------------|---------------|-----------------------------------------------------------|
| `iam_event_names`                           | The list of event names to monitor for Identity and Access Management.                              | `list(string)` | `["DeleteGroupPolicy", "DeleteRolePolicy", "DeleteUserPolicy", "PutGroupPolicy", "PutRolePolicy", "PutUserPolicy", "CreatePolicy", "DeletePolicy", "CreatePolicyVersion", "DeletePolicyVersion", "AttachRolePolicy", "DetachRolePolicy", "AttachUserPolicy", "DetachUserPolicy", "AttachGroupPolicy", "DetachGroupPolicy", "CreateUser", "DeleteUser", "UpdateUser", "CreateGroup", "DeleteGroup", "UpdateGroup", "AddUserToGroup", "RemoveUserFromGroup"]` |
| `cw_log_group_name`                         | The name of the CloudWatch log group storing CloudTrail logs.                                      | `string`      | n/a                                                       |
| `cw_metric_filter_namespace`                | The namespace for the CloudWatch metric filter.                                                    | `string`      | `IAM/Monitoring`                                         |
| `cw_metric_filter_value`                    | The value to publish to the CloudWatch metric.                                                     | `string`      | `1`                                                       |
| `cw_metric_filter_alarm_comparison_operator` | The comparison operator for the CloudWatch metric filter alarm.                                     | `string`      | `GreaterThanOrEqualToThreshold`                          |
| `cw_metric_filter_alarm_evaluation_periods` | The number of periods over which data is compared to the specified threshold.                       | `number`      | `1`                                                       |
| `cw_metric_filter_alarm_period`             | The period in seconds over which the specified statistic is applied.                                | `number`      | `300`                                                     |
| `cw_metric_filter_alarm_statistic`          | The statistic to apply to the alarm's associated metric.                                           | `string`      | `Sum`                                                    |
| `cw_metric_filter_alarm_threshold`          | The value against which the specified statistic is compared.                                       | `number`      | `1`                                                       |
| `cw_metric_filter_alarm_actions`            | The list of actions to execute when the alarm transitions into an ALARM state.                      | `list(string)` | `[]`                                                      |

## Simple Example
```hcl
module "aws_monitoring_iam" {
  source                         = "path/to/module"
  cw_log_group_name              = "the-cloudtrail-log-group"
  cw_metric_filter_alarm_actions = ["arn:aws:sns:region:account-id:sns-topic"]
}
```

## Advanced Example
```hcl
module "aws_monitoring_iam" {
  source                                     = "path/to/module"
  iam_event_names                            = ["DeleteUser", "CreateGroup"]
  cw_log_group_name                          = "the-cloudtrail-log-group"
  cw_metric_filter_namespace                 = "IAM/Monitoring"
  cw_metric_filter_value                     = "1"
  cw_metric_filter_alarm_comparison_operator = "GreaterThanOrEqualToThreshold"
  cw_metric_filter_alarm_evaluation_periods  = 1
  cw_metric_filter_alarm_period              = 300
  cw_metric_filter_alarm_statistic           = "Sum"
  cw_metric_filter_alarm_threshold           = 1
  cw_metric_filter_alarm_actions             = ["arn:aws:sns:region:account-id:sns-topic"]
}
```

## Changelog
For a detailed list of changes, please refer to the [CHANGELOG.md](CHANGELOG.md).

## License
This module is licensed under the [MIT License](LICENSE).