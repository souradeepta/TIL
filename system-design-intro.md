# System Design Introduction



When you are asked a System Design Problem in your interview then you need to take care about some of the things :

- Understand the problem clearly. When you are told to design a system or a service then you should not jump into designing the system instantly. Try to `ask some essential questions` to the interviewer like what are the features he wishes to have in the system. A system can have a lot of features and it’s not necessary to have each and every one of them in your system. So ask the interviewer about it. Be clear about what you need to design before actually designing it in the first place.
- Don't be afraid to `make some assumptions`. When you are designing a system you will have to make some assumptions which will affect your system heavily. Like how many users will be using your system, how many read and write requests will be made. It’s always smart to make some reasonable assumptions before designing the system. Your interviewer may tell you to reconsider your assumption if you are way off the charts.
- Be confident while you solve the problem. You have the features which you need to put in your system, you have made the calculations which you need to take care of while designing the system. Now it’s time to actually design the system. Be confident while designing it, be confident when you choose a particular service or process during the design. Try to `discuss the advantage of choosing the path and process`, discuss the problems you are solving, bottlenecks you are avoiding and many more. And be confident as you solve.
- Take the interviewer’s words seriously. If your interviewer is stressing about some features you need to put in the system then you should make your first priority to have them. If your interviewer tells you to reconsider your assumption then it’s time to move a step back and `reconsider your assumptions`. Your interviewer wants you to design it correctly and hence if he states something then it’s wise to consider it.

There are many conceptual terms one should know about in System Design. These terms are described below in detail.



## Reliability

For a System to be reliable there are some expectations which are needed to be fulfilled :

- The System performs the function as expected.
- It can handle errors. Should be able to tolerate errors and faults which can arise in an unexpected way.
- Its performance is efficient under an expected load and data volume.
- The system must be secured.

Our system should be fault-tolerant. The things that can go wrong are called `faults`. These are the errors which can arise in a single component of the system. It is hard to make a system 100% fault tolerant. But we should try to build a system which can tolerate those faults that can turn into failure. `Failure` can be defined as the process of the whole system becoming unable to provide expected services.
There are various faults which can cause a System to be unreliable.

1. Hardware Faults : These are the faults which can arise due to hardware failures like RAM Failure, Hard Disk crash, Power Grid failures or someone putting wrong network cables. We can use disks in RAID configuration so that a backup is available when something goes wrong, having dual power supplies, having generators as a backup power supply.
2. Software Faults : These are the faults which arise in the software components and can cause more damage than hardware failures as these faults mostly remain undetected until they are triggered by an unexpected situation. To avoid these kinds of failure we can perform a thorough testing, measuring, monitoring and analyzing system behavior before putting them into production.
3. Human Faults : These are the faults which can occur due to human negligence. We can avoid this by doing a thorough testing of our system. Providing a full fledged sandbox account to the developers so that they can test all the possible circumstances. Decoupling the fault prone components with the rest of the system.



## Scalability

Scalability refers to the ability of your system to cope up with the increasing loads. There can be chances that your system working reliably at present may become slow or possibly crash when the load increases. There can be a chance of degradation in the performance if the number of users on your system increased from 1,000 to 100,000. You can add further computing resources or may increase the computing power of the underlying resource in order to cope up with the increasing load.
In order to discuss the scalability problem we need to mention or describe the load, this can be done through the use of load parameters. There are various load parameters which can be used to describe it :

- Number of requests per second made to your web server.
- Hit rate of your Cache.
- Ratio of read and write requests made to your database.
- Number of active users on your website.


When you are done with describing your load you can monitor what happens when this load increases. The performance of your system can be defined through various parameters like:

- Throughput : The number of requests processed per second by the server.
- Response Time : The time interval between the user sending the request to the server and getting the response back.



## Maintainability

The major cost of the system goes in its maintenance and not in its initial development. Keeping the system operational, Fixing bugs, Adapting to new platforms, adding new features and many more. You should design the system in such a way that it does not make much of a deal during the maintenance. Your system should be :

- Operational : It should be easy for the operation teams to keep the system running. It’s said that “Good operations can often work around the limitations of a bad software but good software can never work reliably with bad operations”.
- Simple : Your system can be simple allowing the new engineers to understand them by removing as much complexity as possible. There are various symptoms of complexity like tightly coupled modules, inconsistent naming and terminologies, tangled dependencies etc.
- Evolvable : Make your system evolvable so that it could be easier for the Engineers to make changes to the system in future, adapting them to unanticipated use cases as requirements change.