[![Build Status](https://travis-ci.org/nielsdenissen/flink-avro-state-recovery.svg?branch=master)](https://travis-ci.org/nielsdenissen/flink-avro-state-recovery)

# Flink Avro State Recovery

Example project on how to do state recovery in Apache Flink using Apache Avro.

# Data schema updates

We're using Avro schema's to update state in Flink. This enables us to evolve data as business requirements change.
As a result, there is a procedure you need to follow in order to update any data in Flink:

1. Alter your case class
2. Move the current Avro schema to the history list of avro schemas for the case class. (this is needed to resolve old
schemas when decoding later on)
3. Add the new Avro schema in the corresponding case class' companion object with support for the evolution done 
(e.g. default value when adding field).
4. Alter from/to GenericRecord functions to cope with the added field. Ensure that a GenericRecord generated by the old
case class can be parsed by the new case class. (You will get the old GR from Flink atm)
5. Add old case-class to test data and write tests to check evolution through serialisation and when directly transforming
GenericRecord (e.g. dataV2.fromGenericRecord(dataV1.toGenericRecord)).
