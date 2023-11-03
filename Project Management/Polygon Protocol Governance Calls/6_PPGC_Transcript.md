## Polygon Protocol Governance Call (2023-08-10 16:06 GMT+1) - Transcript

### Attendees
Cameron Morris, David Silverman, Dimitri Nikolaros, Fireflies.ai Notetaker Hyung-K, George Serntedakis, Harry Rook, Jackson Lewis, Jason Windawi, Jerry Chen, Jerry Chen's Presentation, Joonkyo Kim, Keefer Taylor, Lyes Lefgoum, Matt Nathanson, Michel Muniz, Mike Brucken, Mirella Guglielmi, Nimit Bavishi, Paul O'Leary, Pratik Patil, Pratik Patil's Presentation, Quentin de Beauchesne, Sandeep Sreenath, Scott Lilliston, staking 4all, Tobias Geiger, Vaibhav Jindal, Vincent Taglia, WebDev StakeWorks, Yuling Ma
# Transcript

#### **This editable transcript was computer generated and might contain errors.**

**Harry Rook:** Okay, thank you all for joining as the sixth Ppgc. So, yeah, it's nice to get a bit of cadence to these calls now. And good to see many faces again.
So yeah, just to kick things off a little bit of housekeeping with the upcoming discussion on 2.0. I think we are. Potentially going to increase the cadence of these calls to every two weeks just so we can fit in all the discussion around 2.0 because I think that potentially going to come thick and fast in the upcoming months. So yeah, just some extra air time for you all to ask questions and we can present some of the proposals in relation to those. So, yeah, every two weeks going forwards. Let me just link the agenda in the chat. If you all
Yeah, just to give a quick overview in terms of what we're discussing today. Account  Abstraction is what  Pratik is going to be talking about along with Sandeep Hip 16, which is block, STM and parallelization, I believe we've got Jerry on the call, who's going to be discussing that? And then PIP 11. So on this one there's been a few changes post audit. So the spec and then I believe we're also going to talk about some ideas around potential hard fork in the future. And I believe some people are talking about that one. And then to finish off these upstream changes from the Shanghai Fork that we'll discuss as well.
After that we'll do a Q&A and you guys can ask any questions that you may have. So yeah, to kick it off, Pratik or sandeep. Do you guys want to run through PIP 15? Yeah, feel free to take it and go ahead.

**Pratik Patil:** Hello everyone. So Good 15, basically added support for the bundle transactions which were introducing EIP 4337. So now EIP, 4737 introduced by letters and the task of Bundler is to basically bundle the user operations into a single transaction and send this single transaction to the network. What happens is that there is a delay between the community transaction, on the burner Spain. And then the final inclusion of these transactions in the block. what this delay into
It is introduced that this bundler transaction is targeted for this entry point contract. this delay In between this time, there can happen. Some different transactions pointed towards the same contract which might change the account storage of this particular contract. And then it will invalidate the bundle transaction that was sent by the burger and this will result in the bundler losing the transaction. So wonder does not benefit from this particular scenario. The second way burners bundle bundles can basically lose his transaction fee by his transaction. Getting rewarded on chain is when MBB board basically from this particular transaction.
This again, changes the account storage at that particular location. And hence, when this particular transaction is up for execution, when a minor picks this transaction up to include in a block, it gets you ordered on Chen and hence the burners lose the transaction fee, for that particular transaction. Now to tackle this problem, ethereum Researchers basically introduce this new endpoint targeting specially for the burners, which is this eat, and their scores and draw transaction.
Additionally, the transaction by the bundler is called a conditional transaction. Now, this endpoint is similar to the current endpoint that we have, for sending transactions, which is eid. And that's a good underscore, same raw transaction, but with one additional parameter. This parameter basically says that it only includes this particular transaction, if the fields specified in this particular parameter are valued. what these exact things in this parameter are, let's look at that. So the first one is the block range, the TIMESTAMP range and something called a known account.
So what this means is that but the minor should include this particular transaction. Only if it falls in between this particular block range and timestamp range and make sure that everything specified in known accounts is known accounts is nothing but a map of account with expected storage. In this case, it will be your age. the storage add of that the counselor is of that particular entry point contract. So basically whenever someone print runs this particular transaction or some other transactions changes the storage this particular fields in known accounts will no longer be valid and hence the
The transaction will basically not even go in the execution phase because before going into the execution phase, the minor will take all these conditions and only proceed. If all these conditions are checked and are met. Hence, the reward of this particular transaction on chain does not happen because of this invalid state because of the storage being changed. Hence, this support is less as they do not lose their transaction fees.
The thing to note here is that these conditional transactions API is basically this e Thunder score, send raw, transaction conditional, API is privileged and by privileged, what I mean is that in order for a bundler to send his transaction to the network or basically if you want to send his transaction over this particular EPA and point, then He must send this particular transaction directly to the minor and not to a century load because this particular transaction will not be broadcasted to the network. If you want to broadcast this particular transaction to a network with this additional Metallica, it would require hard work and right now we don't want to take that particular part. Therefore,the burner food only sends this particular transaction to a minor and this basically requires a directional trust where a minor trust that abundant will not spam is not for transactions. And Lord, and Bundler trust that the minor will not central to his transaction. Which is, I think, in the law. So transaction people, Yeah, so this is the thing about the mutual trust or wide rational address.
So basically this is how a request from the binder would look like it will contain the sine transaction and this additional metadata that I talked about where it Mentions the block range, the timestampage as well as the known accounts map. Yeah, this is a sample.
A simple response would be the transactional hash if it is a valid transaction, if the fields are invalidated on the API level, then there will be appropriate error messages with the code saying the transaction does not meet. basically the block range is too low or too high, basically, or it does not fall in the block range or TIMESTAMP, range or the loan accounts are not valued We have. So, basically, this is just adding an extra API so it does not require a hard for therefore it is backwards compatible. There are a few security countries considerations, which are not basic. Yeah, basically, this one actually is present in the current architecture as well. Second thing is that in order to make sure that a particular bundle transaction should not consume more CPU users, we have put a limit on the maximum number of slots and accounts in the Known accounts structure and we have limited it back to 1000. Yeah, we have a reference implementation of this ready. You can check it out on this particular PR and the specification of the Ethereum Foundation. Researchers are here, in this one and this is the ERC 4337.
Yeah, that is pretty much it. If you have any questions, I would be happy to answer that.

**Harry Rook:** Thank you Pratik, it may be worth touching on a few of the concerns that were in the comment section on the Forum. I think you did mention that briefly, so the proposal was amended
To basically address some of the concerns that were in some of the comments. I don't know if you want to briefly go over.

**Pratik Patil:** Yesterday, we updated the PIP right. So that basically addresses some of those concerns basically So it is not mandatory for evaluator to accept this translations, Because in order for a valuator to receive this transaction, it is up to him whether he should allow some minds to send transaction directly to him because the only the abundant can stand Conditional transaction is by having a direct connection with the minor. so, if the minor does not want to
Deal with bundle transactions. Then he should not have a connection with anyone that's basically. And if a bundler wants to send a conditional transaction to the network, then it is his responsibility to basically convince the validator To to send him transactions. Maybe through RPC or gold request and whatever it is.

**Paul O'Leary:** We work with bundlers to see basically how the correct role I would be. There's a lot of details and complications to it. 

**Harry Rook:** Also, Thank you, Paul. Thanks Pratik. Yes, there's no question. I think we can move on to Pip 16. Yeah, there's no question. We can move on.

**Jerry Chen:** Let me share my screen. Yeah, just one second. So PIP 16 is basically focusing on parallel execution of transactions Basically, we introduce parallel execution. And how it works is basically we optimistically execute transactions. And we derive the dependency between transactions on the flight when executing these transactions. However, there's some sort of inefficiency, in these optimistically execution due to, there's conflicts between race conditions between these transactions. so that sometime
and computation resources wasted in this manner. So what we want to improve in this PIP, 16 is that For the minor, when they produce a new block We ask the miner to figure out the dependency between these transactions in a block and Put these information in the block header so that when Rest of the network is trying to validate this block. They can take this information and execute transactions in parallel and without any sort of race condition and conflicts, So by transaction, dependency here, give a pretty simple example, here is that for example, a depend transaction b depends on transaction. when a transaction, for example, increases the balance of an account while a transaction reads the balance of that account, right? So in this sense, transaction B depends on transaction. and we have to execute the first transaction, before executing transaction B. And if we execute them in parallel, the result of the transaction is false. And in Word zero four zero. We have these validation process to make sure that even if both transactions, what executed at
Transaction B's result will be invalid and we'll execute transaction B. And in these way, if we have these dependency information, then the executor or the scheduler will basically ask transaction B to wait for the result of transaction and until the result is become available then we'll execute transaction be so, Going very briefly over the formats of the transaction dependency. It's pretty straightforward. It's basically a two-dimensional array where the first dimension is representing the dependency of each transaction and
The second dimension is basically a list where this particular transaction depends on, So we encode this information in the block header. Now they're many different solutions to how we can encode or put this information in block header and the past that we chose was to use this existing field called actual data. So these extra data previously looks like these in our block header and basically has extra vanity and by dealer set and extra seal
It previously was what we use these fields to store the two most important information which is the value their set, and the signature of the minor. Since these field doesn't have, a restriction on the size so that we can add more information. Station as we want. Here's the full new formats and you can see the middle part of The bytes is replaced by a combination of values. There is a transaction dependency and encodes this information here. And, when we decode, we just use iopd codes to get this information out of the battery.
And This is an example of how this array will look like, if we have six transactions and then a bunch of dependencies are listed here. so, I'll get the rationale because it might take more time. Then we need so, the most important part is some security considerations on this topic. For example, valuables can generate basically Transaction, Dependency When they create the block, although it's rare or it's not very likely to happen, but would this happen. We need a solution to prevent the black data who validates the blog, running into error states or we need to kind of Have the validator. Or whoever valued this block to be able to surpass these incorrect dependencies, right? So, we have these internal mechanisms of, even by executing the transaction with the other at provided by the transaction dependency metadata. We still verify after the execution is complete, every single transaction is reading and writing to the correct path of the slots and making sure that there's no conflicting dependency happening. And if there is then the transaction will. Go back to be executed. so, Another two scenarios are similar to incorrect dependency but more specific out of range. Problem is that the transaction dependency is for example, there's only less than 10 transactions, but, in these metadata transactions 10 show up, which is non-existent. So in this case, before even executing the transaction, the valuator will verify that there's no index that is larger than the size of the transactions in that block and the circular dependency is saying for example transaction 1 depends on trend transaction 2 and then transaction Two is depending on transaction one. Obviously these will lead to a deadlock in execution. So we will also have a check on these social independencies to make sure that these won't cause any dead log in the execution path. Yeah, so that's basically the overall view of this transaction dependency metadata approach. So yeah, we also posted these to the form. So if you feel free to ask any questions or, if you have better ideas, everything is welcome.

**Harry Rook:** Thank you very much Jerry. Does anyone have any questions?
Dimitri. Go ahead.

**Dimitri Nikolaros:** Hey guys, I have the understanding that the validator if transactions are one dependent on the other one depends on the other or not and then it'll encode into better read and later will be the order by question is, How does it determine it? How does it know at what stage does it verify and how does it know if a transaction which is a fetch, data is dependent to transfer assets or something right? How does that work?
Jerry Chen: Yep. Thanks. This is a really good question. So, what's happening during the block creation? Was that when the minor executed every single transaction, it records the read and write slots of every single transaction. So, for example, if the first transaction rights to An account balance you'll get recorded. So basically these will be stored internally in a data structure and when the minor completes all the transactions it will use this information to Drive the final dependency. So basically you can think of each transaction will have a list of bright and reads. X are right and read slots and then we just simply compare if there's any overlap between the read and the right slots of each infection. And in the end we'll basically simplify these transactions. Basically, for example, if a transaction depends on transaction one and transaction, three depends on transaction 2. Then we'll only expose information saying transactions Three depends on transaction 2 and then get rid of transaction one in this dependency graph because it is additional information that we don't actually need because, trajectory is already depending on transition too. So there's no point in adding transaction one to these graph.

**Paul O'Leary:** Yeah, I just say it's worth pointing out. We've done a blog post about this before basically. But the technique that we use here is kind of derived from a technique called Block STM that was proposed and implemented by Aptos. And again, this is public knowledge. We've tweeted about this and they're happy about us adapting a series. So, if you want a much more detailed explanation of the technique, then you can find the research basically, on block SDM, what this represents and this is already. I mean this technique that we use the black sdm, kind of dynamically determine dependencies is already on main net and has already shown kind of huge advantages for block execution and propagation What this is is an additional refinement because what we realized as we went through the implementation is that expected of execution, part of block SDM, encourage some overhead and the only in any system basically the only actor that actually needs to incur that over

**Harry Rook:** Great, Any more questions. If not we can move on to Pip 11. Okay, no, it's no worries. Cool. Okay, we can move on then. We have Vaibhav or Sandeep on the call, feel free to take it away.

**Vaibhav Jindal:** Thank you. So we did the audit over PIP 11 and based on our audit we made some minor changes. Based on the minor changes we implemented a new version to Heimdall and Bor.

**Sandeep Sreenath:** Yeah, described some more context the reason for having A certain gap between two milestones is because there were a few race conditions possible, where multiple milestones get proposed in, immediately one block after the other or could be within the same block and a bunch of notes could be on a particular or could vote. Yes. For one of the milestones and also what is for the other one? In which case it would release the rear lock for the first one. So, yeah, in such cases there was some unexpected behavior which was possible. and anyway, given that we have a minimum length for milestone which will be I think is it both at 16 board blocks So yeah it doesn't really make sense to anyway have more and more milestones proposed within a span of two or three blocks because even at 12 second or 12 or blocks would correspond to around 24 seconds which would correspond to around three heimdal blocks. Anyways, considering that Him Dial blocks Around seven, seven and a half to eight seconds. So yeah, this is the reason, this was a pretty simple fix and it also kind of eliminated all the race conditions that were possible.
Yeah, any questions?

**Harry Rook:** Yeah, let's move on. Thank you Vaibhav and Sandeep. Do we want to discuss testing and how that's going for milestones? I don't know if we expect complete or, If you want to discuss that in detail,

**Sandeep Sreenath:** So I think in the previous ppgc we briefly covered the latest changes from upstream get so we have been working on it for more than a couple of months. Probably three months already and the good news is that we have the beta release out earlier this week so basically this contains almost 10 to 12 months worth of upstream changes coming from GoEtherium. And yeah it also includes Shanghai changes. Obviously, it took a lot of time because there were a lot of conflicts that we had to resolve along the way and the client was not even starting up. So we had to do a lot of fixes and the tests were failing all over the place. It took almost a couple of weeks to even resolve all the test cases and I had some additional test cases so that all our changes which are on top of the kept changes are also covered. So yeah this was quite intense work by the team for the last couple of months. And yeah, finally we have this release, which we are already testing out on my way, isn't it? Yeah. I would also some
To try it out on your notes as well. If you're running a Mumbai node, So yeah, the version is 050 beta 4. This seems to be pretty stable. I was released a couple of days back and we have already upgraded our notes and are monitoring it actively. So the rough timelines is that we want to finish testing this on our internal nodes and also proudly announce regarding this probably sometime next week. And yeah, and then soon, after this, we will be basically merging the milestones code, the people which were explaining on top of this. And since again, these are recent changes and milestones, we have been working on it for quite some time. So there will be some additional merge conflicts that we are anticipating when we actually merge this code, but hopefully we should be done with all that and another five and a round of testing the next couple of weeks.
 Yeah, at a very high level or, a very rough estimate, we are targeting the end of August for, the milestones are to be out, which would obviously, require a hard fork on him Doug. And so yeah, this is on Mumbai, so, if everything goes, then we might be ready with the main network release as well as a hard fork or towards the end of September. But again, it depends on how the testing goes and How it works on Mumbai as well?

**Harry Rook:** Does anyone have any questions on any of that?

**Sandeep Sreenath:** Yeah, just a minor point which I probably missed. So I mentioned that we have these Shanghai changes in the release, but, we're not planning to activate the Shanghai block yet because we just want to bake in these changes, test out everything. And then probably once the client code is tested. Then we will start, discussing the hard fork  which enables milestone changes. So yeah, just a minor clarification that although the changes are there in the code ways, we are not really activating it for now.

**Harry Rook:** Perfect, thank you Sandeep. Okay, then, I think that covers everything for today. How are we doing on time a little bit early? Yeah, if anyone's got any questions even in relation to any of the pips. So agenda items and feel free to ask, we've got a couple minutes to spare. So yeah, I'll open the floor if anyone wants to ask you a question.
Okay, I think we've covered everything for today. just a reminder. These are probably going to be occurring every two weeks going forwards. So expect the next one to be scheduled for the 24th of August. And at that point, there'll be more of a focus on some of the polygon 2.0 proposals. Yeah, that'd be interesting to discuss with you guys. Yeah, thank you all for joining and yeah, I'll see you all on the next call.

Meeting ended after 00:39:02 👋