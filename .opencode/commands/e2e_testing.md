---
description: Do End-to-End Testing of the UI, backend and other micro-services
---

We really need to have totally complete, totally comprehensive, granular, perfect end to end testing coverage without ANY mocks or fake data, fake api calls, etc., that prove that our entire pipeline from start to finish works perfectly in a provable, ultra rigorous way. That means the raw data coming in from the various API services for EVERYTHING (not just one or two fields) for a bunch of test cases. Basically, the WHOLE thing, from "soup to nuts".

Start all the server and verify that the deployment worked properly without any errors (iterate and fix if there were errors). Then visit the live site with playwright-cli as both desktop and mobile browser and take screenshots and check for js errors and look at the screenshots for potential problems and iterate and fix them all super carefully!

