# Graphite TSDB

## Clean up Carbon metrics

We encountered a full disk and the following articles were more than helpful in
resolving it.

First we did clean up the whisper database to get at least some space on the
disk: <https://sysadmin.compxtreme.ro/clean-up-whisper-database/>

And then we changed the data retention period and used a tooling to apply the
new data retention to the existing metrics:
<https://www.linkedin.com/pulse/how-resize-already-existing-whisper-files-datapoint-panneerselvam>
