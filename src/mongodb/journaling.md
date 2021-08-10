# Journalling

Every production MongoDB instance should run with journalling enabled. If you don't have journalling enabled and your server or mongod process crashes MongoDB will not be ensure data integrity.

MongoDB creates a journal (a dairy or daily record of events) for every write that includes the exact disk location and the bytes that changed in the write. Thus if you have a crash in the server, the journal can be used to replay any writes that have not yet been written to the data files.

By default MongoDB data files are flushed to disk every 60s. MongoDB also uses memory mapped files for the journal. By default the journal is flushed to disk every 100ms earlier than the actual data when written to disk. This is called write ahead logging to on-disk journal files to rescue in case of failure.
