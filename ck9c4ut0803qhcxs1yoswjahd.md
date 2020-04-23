## RxSwift, lesson 3

Semaphore
* Tells next async call to wait until the current one is finished. In case they all hit the same variable, you'll get weird results if you don't wait til one is finished.

` if let base = readLine()`
`readLine` = read from the terminal, so you can type straight into the terminal
