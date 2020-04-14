# Single vs double precision

First of all **float** and **double** are both used for representation of fractional numbers (number with decimal). So, the difference between the two from the fact with how much precision (accuracy) they can store the numbers.

> For example: I have to store 123.456789 One may be able to store only 123.4567 while other may be able to store the exact 123.456789.

## What is precision

The precision indicates the number of decimal digits means the number of digits after `.` can safely be used or stored in memory.

## Single precision

* Float can accurately store about `7-8 digits` in the fractional part.
* Single precision floating point arithmetic deals with 32 bit.
* Is widely used in programs requiring less precision (accuracy) and wide representation.

## Double precision

* Double can accurately store about `15-16` digits in the fractional part.
* Double precision deals with 64 bit.
* Finds its use in the area to scientific calculations, where precision is all that matters and approximation is to be minimized.
