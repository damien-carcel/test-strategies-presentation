# Testing Strategies

## Plan

### How to do better

- Reminder of what makes a "good" test ⇒ F.I.R.S.T. (Medium article that summarizes the topic well).

- Recall the different "doubles" that exist (in-memory, stub, mock) and mocks are not the only solution.
  - But when to use what?
  - In memory => transition with hexagonal architecture and port/adapters.

- Reminder of hexagonal architecture via a diagram to introduce the notion of "test boundaries."
- The different types of tests: unit, end-to-end, integration, acceptance.
- Unit tests are fast and scalable.
- Presentation of the concept of "acceptance tests":
  - they focus on business, so are critical, but must be fast. They are kind of higher-level unit tests.
  - Calling directly the use cases and using an "in-memory" adapter for the "controlled" ports (on the right side of the hexagon): DB, file system, time, etc.
  - The tests themselves can be seen as an adapter for the "controlling" ports (on the left side of the hexagon).
- Integration tests complement acceptance tests by testing all implementations of controlled adapters (e.g., Doctrine repo + its in-memory double) via the same test.
  - Allows full confidence in acceptance tests since the "in-memory" adapters are reliable.
- End-to-end tests that test Symfony controllers and commands should be reduced to the bare minimum, only on the most critical, valuable paths.

### Tradeoffs

- Require a good code design, with clear boundaries between domain and infrastructure.
- Require a systematic "in memory" implementation of controlled ports => more work.
  - If correctly designed, those ports do very little, and so are quick to test and implement.

## Examples

- Simple use case of creating a user account:
  - [Bonus] Send an e-mail to the user to notify them their account was created.
    - Dispatch a "UserCreated" event
    - Use an event subscriber to send the e-mail.
  - Tested with both unit tests and acceptance tests.
  - Two flavors of unit tests: one with a real, in-memory repo (similar to acceptance tests) and one with a mocked repo instead.
- Two evolutions to be done each in a dedicated PR (will allow linking them in the presentation):
  - Additional feature: Check for e-mail uniqueness in persistence, throw an exception when e-mail is already used.
    - No change at all for acceptance tests or unit tests with in-memory repo.
  - Refactor the repository to throw an exception when a user is not found instead of returning null.
    - Minimal changes for acceptance tests or unit tests with in-memory repo (just on the final assertion).
    - Will break existing unit tests with mocked repo as we need to mock the call to get the user by e-mail, whether it returns a user or throws an exception.
