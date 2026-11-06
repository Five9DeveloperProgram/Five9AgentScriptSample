# Five9 Agent Script Sample

A small, dependency-free Five9 Agent Script demonstrating dynamic call variables, a tabbed script, contact updates, and callback scheduling through Web2Campaign. Plain HTML, CSS, and JavaScript: no jQuery, framework, package install, or build step.

## Setup

1. In [index.html](index.html), set `F9domain` to your Five9 domain name and `F9list` to the target calling list. The list defaults to `Web2Campaign`.
2. Customize the Script tab and the contact fields for your workflow. Field names must match your Five9 contact fields.
3. Deploy the HTML using your Five9 Agent Script workflow. Preserve the `@...@` variable placeholders and their element IDs so Five9 can populate them.
4. Test with a non-production domain/list and test contacts before using real customer data. Submissions can modify CRM records and calling lists.

Open [index.html](index.html) directly in a current browser to preview the layout and tabs. Five9 variables remain placeholders outside the Agent environment; live data and callback submission require that integration. Serve over HTTPS when hosting the sample.

## Behavior

- Contact fields populate as Five9 supplies call variables, without overwriting agent edits. Contact updates are disabled when the call-end variable is populated. This sample assumes one call per page load.
- Contact updates use `number1` as the Web2Campaign key. Changing the primary number changes the lookup key; this is not an update by immutable contact ID.
- Callback scheduling uses the call's original `Customer.number1`, not an unsaved edit in the contact form. It remains available after call end.
- Callbacks must be in the future, between 08:00 and 22:00 inclusive, in five-minute increments. Eastern, Central, Mountain, and Pacific use the New York, Chicago, Denver, and Los Angeles time-zone rules, including daylight saving time. These choices do not cover regions with different rules, such as Arizona.
- Callback timestamps use `MM-dd-yyyy HH:mm Z` with an explicit numeric UTC offset. Invalid or ambiguous local times are rejected.
- Requests are asynchronous, prevent duplicate submissions while pending, and report success or failure in the submitting tab. Tabs support arrow keys, Home, and End; fields use native browser validation.

## Integration Notes

The sample retains the Web2Campaign `AddToList` **GET** request and its HTML response fields (`F9errCode` and `F9errDesc`). Parameters are URL-encoded and response messages are rendered as text. The browser must be allowed to read the response by the endpoint's CORS policy. If your deployment does not permit this, use an approved server-side integration; do not disable browser security or use `no-cors` to hide the failure.

GET parameters include contact data and may be recorded in server or proxy logs. Review logging, access controls, and data-handling requirements before production use. Do not embed API credentials or other secrets in this page. Five9-injected values must be HTML-escaped by the system that substitutes the placeholders; reading them with `textContent` cannot secure raw HTML inserted before this script runs.

Requests time out after 15 seconds. A timeout or network failure does not prove that Five9 rejected the update: verify the result before retrying. Validate live Web2Campaign permissions, response handling, and callback scheduling in your own Five9 environment.

## More Interactive Examples

For other use-case samples and more interactive agent and supervisor experiences, see the [Five9 Agent Supervisor REST API Starter Kit](https://github.com/Five9DeveloperProgram/Five9-Agent-Sup-REST-API-Starter-Kit).
