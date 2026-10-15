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

# Cybersecurity

# Data

# Misconception in My Track

# Open Question
