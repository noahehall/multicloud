# Budgets

- Improve planning and cost control with flexible budgeting and forecasting
- tracks and takes action on your AWS cost and usage

## my thoughts

## links

- [landing page](https://aws.amazon.com/aws-cost-management/aws-budgets/?did=ap_card&trk=ap_card)
- [getting started](https://aws.amazon.com/getting-started/hands-on/control-your-costs-free-tier-budgets/)

## best practices

- set custom budgets based on your costs, usage, reservation utilization, and reservation coverage.
- budgets on a recurring basis so that budget alerts don't unexpectedly stop coming.

### anti patterns

## features

- Track costs, usage, and coverage with custom budgets.
- Create custom actions to prevent overages, inefficient resource use, or lack of coverage.
- set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount

### pricing

- Your first two action-enabled budgets are free
- Afterwards each subsequent action-enabled budget will incur a $0.10 daily cost.
- Each report delivered will incur a cost of $0.01.

## basics

- requires approximately five weeks of usage data to generate budget forecasts

### Alerts

- for both actual (after accruing) and forecasted (before accruing) spends.
- Forecast-based budget alerts are sent out on a per-budget period basis
- up to 10 email addresses and one Amazon SNS topic per alert

### Cost Budgets

- fixed target: track all costs associated with your account
- variable target amount: each subsequent month growing the budget target by XXX percent each month
  - configure your notifications for YYY percent of your budgeted amount and apply an action
    - e.g. automatically apply a custom IAM policy that denies you the ability to provision additional resources within an account.

### Usage Budgets

- fixed usage: forecasted notifications to help ensure that you are staying within the service limits for a specific service.
- daily utilization: aka coverage budget;

## considerations

## integrations
