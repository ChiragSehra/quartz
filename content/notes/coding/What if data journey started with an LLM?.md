---
tags:
  - "#coding"
title: What if user data journey started with an LLM?
by: Chirag Sehra
date: 2023-01-15
enableToc: true
---


1. User Interface
	1. User logs in the application
	2. Dashboard displays options for data onboarding tasks.
	3. User selects the desired target database from a pre-configured list (Ex: MYSQL, PostgreSQL, Google BigQuery)
	4. Depending on chosen database, the UI dynamically adapts to display relevant data fields specific to database schema
	5. Two data input options available: Fill up a form or File Upload.
2. Data Preprocessing
	1. Upon user selection of the target database and data input method, the data is sent securely to the chosen LLM API.
		1. If form input is chosen in the previous step
			1. LLM analyses each form field and attempts to identify the data type, example: text, number, date)
			2. LLM extracts key information from text fields (e.g name parsing, identifying city etc.)
		2. If form input is chosen in the previous step
			1. LLM analyses the uploaded file structure (columns, datatypes) and attempts to identify the headers. (if present)
			2. LLM performs basic data cleaning like removing leading/trailing spaces, converting text to lowercase.
3. Data Mapping
	1. Based on LLM analysis, the UI displays a table with 2 sections.
		1. User data: shows either the pre-populated form fields (with extracted data types) or the first few rows of the uploaded file with identified data types.
		2. Target Database: Shows the pre-configured schema for the selected database, displaying each column name and data type.
	2. LLM suggests potential mappings between user data fields and target database columns based on data type analysis and semantic understanding. These suggests appear as color-coded lines connecting user and database fields (e.g green for confident match, yellow for potential match needing confirmation).
	3. User can review and confirm mappings (Drag -drop). LLM highlights any inconsistencies or conflicts (e.g trying to map a text field to a date column)
	4. User has the option to manually adjust mappings if LLM suggestions are inaccurate.
4. Data Transformation
	1. Based on the user-confirmed mappings, the transformation happen.
	2. Transformation rules are pre-defined based on data types (e.g. converting dates to a specific format, standardising phone number formats etc.)
	3. LLM can be used for complex transformations:
		1. User can flag specific data points for missing values. LLM can suggest potential values based on existing data patterns (with user confirmations)
		2. Suggest corrections for specific validation errors based on context.
	4. Users receive clear error messages with details about any data validations issues encountered. Users can then choose to correct the data or skip specific entries
5. Database Integration
	1. The tool connects to the target database based on pre-configured user credentials.
	2. Transformed and validated data is uploaded in batches to the designated table within the user's database.
6. Success or Error Notification
	1. After data upload, the system performs a final validation check on the database side
	2. User receives a notification based on the outcome:
		1. Success: Confirmation message displays of successfully uploaded records. Users can access the data directly within their database.
		2. Error: Notification details any errors encountered during the upload process. (eg: data format issues etc.) Users can then download an error report with specific details for correction and retry the upload.
7. Logging and Auditing
	1. All user actions, data transformations, and error messages are logged within the application for audit purposes.
	2. User can access detailed logs to track their data onboarding tasks and identify any recurring issues.
8. Additional features to think about later
	1. Scheduled data onboarding
	2. Customizable data transformation rules
	3. LLM training for specific domains: (to know the jargons of the industry)