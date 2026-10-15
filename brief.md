Version: NAFATH-V2
Track: Cybersecurity

# Overview
A telecom operator is launching a 5G service for emergency responders that must stay reliable under congestion, outages, and cyberattacks. The six IT fields connect in one chain: the Software layer must retry failed HTTP requests safely; AI predicts network congestion before it happens; Mobile apps must keep working offline and sync when coverage returns; Networking provides a dedicated 5G slice with priority for responders; Cybersecurity handles attacks across the incident response lifecycle; and Data turns usage into daily per-region reports. Weakness in any one field breaks the chain, so the brief treats them as one system.


# Software
Safe HTTP methods (GET, HEAD, OPTIONS, TRACE) are read-only and do not change server state. Idempotent methods (PUT, DELETE, and all safe methods) produce the same effect whether executed once or many times. POST is neither safe nor idempotent. When a client automatically retries a failed request, only idempotent methods can be retried safely; retrying POST may create duplicate operations, such as double-charging a card or registering an emergency request twice.

Resource: RFC 9110: HTTP Semantics - https://www.rfc-editor.org/rfc/rfc9110

# AI
Overfitting happens when a model memorizes the training set, including its noise, instead of learning general patterns. It performs very well on training data but poorly on new data. Cross-validation detects overfitting by splitting data into K folds: the model trains on K-1 folds and validates on the remaining fold, repeating K times. If validation loss varies widely across folds or rises while training loss falls, the model is overfitting. A separate validation set lets engineers tune the model without contaminating the test set.

Resource: Datasets: Dividing the original dataset - https://developers.google.com/machine-learning/crash-course/overfitting/dividing-datasets

# Mobile
An offline-first mobile app reads from a local data source (for example, Room) instead of waiting for the network, so responders can still view critical information when connectivity drops. Writes go to a local queue managed by WorkManager. When the connection returns, the app synchronizes with the server using pull-based or push-based sync and resolves conflicts with a "last write wins" strategy. Exponential backoff retries failed syncs without draining the battery.

Resource: Build an offline-first app - https://developer.android.com/topic/architecture/data-layer/offline-first

# Networking
Network slicing divides one physical 5G network into multiple isolated virtual networks, each tuned for specific needs like bandwidth, latency, or reliability. A dedicated slice for emergency responders can be built on URLLC (ultra-reliable low-latency communications) to guarantee priority over ordinary traffic, stable connectivity even during congestion, and low latency. This keeps police, fire, and medical teams connected during outages or attacks, when ordinary users compete for the same resources.

Resource: Spain: Orange tests 5G network prioritisation for emergency services - https://digital-strategy.ec.europa.eu/en/miscellaneous/spain-orange-tests-5g-network-prioritisation-emergency-services

# Cybersecurity
NIST SP 800-61 Rev. 3 defines incident response as three active phases: Detect, Respond, and Recover, supported by preparation activities (Govern, Identify, Protect) and continuous Improvement. When a 5G emergency service is attacked, detection uses monitoring and intrusion detection; response contains and eradicates the threat while minimizing damage; recovery restores normal service; and lessons learned feed back into improvement. A dedicated slice limits the blast radius, since attackers cannot easily move to other slices.

Resource: NIST SP 800-61 Rev. 3: Incident Response Recommendations and Considerations for Cybersecurity Risk Management - https://csrc.nist.gov/pubs/sp/800/61/r3/final

# Data
A star schema places one fact table at the centre, holding numeric measures, surrounded by dimension tables that describe entities like region and date. A snowflake schema normalises those dimensions into multiple related tables. For daily per-region usage reports, the star schema is the better fit: it produces simpler and faster aggregation queries, shorter filter-propagation paths, and easier hierarchies for region and time, while the storage savings of a snowflake schema are irrelevant at this scale.

Resource: Understand star schema and the importance for Power BI - https://learn.microsoft.com/en-us/power-bi/guidance/star-schema

# Misconception in My Track
A common belief is that a strong firewall alone stops cyberattacks. NIST SP 800-61 Rev. 3 contradicts this: the publication states that incident response requires preparation across the Govern, Identify, and Protect functions and active work in Detect, Respond, and Recover, with continuous Improvement feeding lessons back into every function. A firewall only supports part of the Protect function. Attackers routinely bypass firewalls through stolen credentials, insider threats, or zero-day exploits, so a 5G emergency service needs layered defence and a tested response plan, not a single device.


# Open Question
How can a dedicated 5G slice be secured end-to-end when the operator relies on third-party vendors for radio, core, and cloud components? I would verify which assurance standards apply and how incidents are escalated across vendor boundaries.

