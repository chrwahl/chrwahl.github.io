---
title: Experimenting with the EventSource API
date: 2021-04-17 20:33:00 +02
tags: ["Node.js","Express","EventSource","JavaScript"]
---

Last week I had supervision with one of my computer science groups. In collaboration with a local company, they create a video chat application using [WebRTC](https://webrtc.org/). I find it fascinating that video and audio can be streamed between clients without requiring any other application than the browser itself. The tricky part of the implementation is not the streaming of video but the signaling between client and server. Researching signaling, I stumbled upon the EventSource API.

EventSource is an API in the browser for connecting to server-send events. This enables the server to push events to the browser. To get some experience with the EventSource API, I created a small project demonstrating the use of EventSource in a basic Express website.
[demonstrating the use of EventSource in a basic Express website](https://github.com/chrwahl/express-eventsource-chat). The code is inspired by: [How To Use Server-Sent Events in Node.js to Build a Realtime App](https://www.digitalocean.com/community/tutorials/nodejs-server-sent-events-build-realtime-app).

It turns out to be pretty easy to implement server-send events using [Node.js](https://nodejs.org/en/). And in my project where I use [Express](https://expressjs.com/), everything can go into one router:


```[.js]
// the server-send event endpoint
router.get('/events', function(req, res, next) {
  res.set({
    'Content-Type': 'text/event-stream',
    'Connection': 'keep-alive',
    'Cache-Control': 'no-cache',
    'Transfer-Encoding': 'identity'});

  // write an initial event as a 'notice'
  let notice = {text:'Connected to the server!', timestamp: Date.now()};
  res.write(event_content(notice, 'notice'));

  // for each of the messages already in the array, write out an event
  // to the stream.
  messages.forEach(message => {
    res.write(event_content(message, 'update'));
  });

  let client_id = Date.now();
  // a new client with id and responce object
  let client = {id: client_id, res};
  // add client to the clients array
  clients.push(client);

  // remove the client when the EventSource sends 'close'.
  req.on('close', () => {
    console.log(`[${client_id}]: Connection closed`);
    clients = clients.filter(client => client.id !== client_id);
  });
});
```

While fiddling around and testing the code, I ended up in a situation where I had reached the limit of connections I was allowed to create in Firefox. This is a known issue [known issue](https://stackoverflow.com/questions/16852690/sseeventsource-why-no-more-than-6-connections), but basically, the solution is to close the browser's connection to the server.

I also watched this video about [SSE vs. WebSockets vs. Long Polling](https://youtu.be/n9mRjkQg3VE) where Martin Chaov explains a compelling use case for server-send events.