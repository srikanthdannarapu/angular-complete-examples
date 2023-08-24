# CD_DTL Oracle Table Enhancement

## Introduction

The CD_DTL Oracle Table Enhancement project aims to enhance the existing CD_DTL table by adding columns that facilitate the control and management of event listeners and features in a dynamic system. This README provides an overview of the project, implementation details, and considerations for using the added columns.

## Table of Contents

- [Features](#features)
- [Implementation](#implementation)
- [Usage](#usage)
- [Pros and Cons](#pros-and-cons)
- [Contributing](#contributing)
- [License](#license)

## Features

The enhancement involves adding one of the following columns to the CD_DTL table:

- **FEATURE:** This column allows controlling various system features. Supported values include `ALL`, `PAN_REFRESH`, and `OUT_BOX_SCHEDULER`.

- **LISTENER:** This column allows managing individual event listeners. Supported values include `NRT_LISTENER` and `ENTITY_EVENT_LISTENER`.

The choice between these columns depends on the granularity of control needed for your system.

## Implementation

1. **Schema Modification:** The SQL queries to add the chosen column to the CD_DTL table can be found in the [schema.sql](schema.sql) file.

2. **Data Population:** The newly added columns should be populated with appropriate default values or NULLs using SQL scripts or database tools.

3. **Consumer Logic:** Update the consumer logic in your application to utilize the values in the FEATURE or LISTENER columns. Make necessary adjustments to handle start/stop actions based on these values.

## Usage

- To start using the enhanced table, follow these steps:
  1. Run the schema modification query from [schema.sql](schema.sql) using your Oracle database tool.
  2. Populate the new columns with default values or NULLs using SQL scripts or your preferred method.
  3. Update your application's consumer logic to incorporate the new columns for managing features or listeners.

## Pros and Cons

### Using FEATURE Column

**Pros:**
- Clear differentiation between features.
- Scalability and extensibility as the system evolves.
- Flexibility to define different stop conditions.

**Cons:**
- Limited to feature control, might not be suitable for independent listener management.
- Consumer logic could become complex as the number of features grows.

### Using LISTENER Column

**Pros:**
- Granular control over individual listeners.
- Simplicity when listeners have distinct stop conditions.

**Cons:**
- Lack of clarity with multiple listeners.
- Maintenance complexity when managing individual listeners.

## Contributing

Contributions to this project are welcome! If you have any suggestions, improvements, or bug fixes, please open an issue or submit a pull request. For major changes, please discuss the proposed changes before making them.

## License

This project is licensed under the [MIT License](LICENSE).
