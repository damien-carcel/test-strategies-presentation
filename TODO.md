# Testing Strategies

## Plan

### Conclusion

Tradeoffs:
- Require a good code design, with clear boundaries between domain and infrastructure (not necessarily a bad thing).
- Multiple sorts of tests that follow this design.
- Require a systematic "in memory" implementation of controlled ports => more work.
  - If correctly designed, those ports do very little, and so are quick to test and implement.
- Not a silver bullet:
  - complex reading queries => no in-memory implementation, the business is IN the query
