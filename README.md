# Outbox Scheduler

The Outbox Scheduler is a component designed to manage the execution of key processes related to the Outbox pattern in your application. It provides the ability to stop the Near Real-Time (NRT) Consumer and the Event Entity Consumer based on a stop flag stored in the database. This feature is useful for controlling the application during deployments and other activities.

## Implementation Overview

The Outbox Scheduler is implemented following these steps:

1. **Create a New Scheduler**: Develop a new scheduler that runs at a predefined interval. This scheduler will handle the logic to stop both the NRT Consumer and the Event Entity Consumer based on the stop flag status in the database.

2. **Inject Configuration**: Inject the `ConfigurableEnvironment` object into the scheduler to access application configuration properties.

3. **Modify Configuration Property**: Adjust the value of the `outbox.scheduler.enabled` property in the application configuration based on the stop flag status:
   - Set to `"true"` to continue running the application.
   - Set to `"false"` to stop the application.

## Changes in `mastercard.outbox.binder.core` Library

To enable conditional execution of the Outbox Schedulers, the following modifications are made in the `mastercard.outbox.binder.core` library:

1. **OutboxMessageProcessor**: The scheduler is updated to run conditionally based on the value of `outbox.scheduler.enabled`.

2. **OutboxMessagePurger**: The scheduler is updated to run conditionally based on the value of `outbox.scheduler.enabled`.

3. **OutboxMessageReProcessor**: The scheduler is updated to run conditionally based on the value of `outbox.scheduler.enabled`.

## Additional Considerations

- Thoroughly test the Outbox Scheduler in various scenarios, including enabling and disabling the stop flag, to ensure it behaves as expected.
- Implement proper error handling and logging mechanisms to provide visibility into the scheduler's actions and potential issues.

## Getting Started

To integrate the Outbox Scheduler into your project, follow the steps outlined in the [Confluence documentation](link-to-confluence-page).

## Contributors

- [Your Name](https://github.com/yourusername)

## License

This project is licensed under the [MIT License](LICENSE).
