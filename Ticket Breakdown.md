# Ticket Breakdown(Clipboard He)

## Ticket 1: Add custom ids for Agents

    Currently, the id of each Agent on the reports we generate is their internal database id. We'd like to add the ability for Facilities to save their own custom ids for each Agent they work with and use that id when generating reports for them.

Acceptance Criteria

```
- A new field "custom_id"
 should be added to th the Agents table in the database.
- A new API endpoint should be created to allow acilities to update the custom_id for an Agent.
- The getShiftsByFacility function should be updated to include the custom_id of the Agent in the metadata returned for each Shift.
- The generateREport function should be updated to use the custom_id instead of the internal database id when generating reports.
 ```

Time/Effort Estimate

```
- Adding custom_id field to Agents table:1hour
- Creating new API endpoint:2 hours
- Updating getShiftByFacility function:1 hours
- Updating generateReport function:1 hours
- Total: 5 hours

```

Implemenation Details

```
- The custom_id field shoud be added to the Agents table as a tring field with a maximum lengh of 50 characters.
- The new API endpoint shoould accept a PUT request with the Agent's internal database id and the new custom_id as parameters. It hould update the custom_id in the database for the specitied agent.
- The getShiftsByFacility function should be updated to include the custom_id field in the metadata returned for each shift. This can be done ny jonining the Agents table with th Shifts table and selecting the custom_id field.
- the generateReport functions should be updated to use the custom_id fireld instead of the interrnal databse id when generating reports. This can be done by replacing the internal databse id with the custom_id in the report generation code.
```

## Ticket 2: Add calidation for custom ids

```
To ensure that Facilities are using valid custom ids for Agents, we need to add validation to the API endpoint that allows Facilities to update the custom for an Agent.
```

Acceptance Criteria
```
- the new APi endpoint should validate that the custom_id proavided is a string with a maximum length of 50 characters.
- If the <!-- markdownlint-capture --> is invalid, the api endpoint should return a 400 bad request respons with an error message.
```

Time/Effort Estimate
```
Adding validation to api endpoint : 1hour
```

Implemntation Details
```
- The new API endpoint sould validate the custom_id parameter using a regular experssion to ensure that it is a string with a maximum ength of 50 characters.
- If the custom_id is invalid, the API endpoint should return a 400 bad request response with an error message explaining the validation failure.
```