# References

For this Cylon example, we're going to use a helper function called
`addObjectRelations`. This method goes through each element in a frame, relating
them in helpful ways. For example, you can find out what `hands` are involved in
a `gesture`, or what `hand` a `pointable` belongs to.

To get started, let's load up Cylon:

    var Cylon = require('cylon');

With that done, we can start setting our robot up.

    Cylon.robot({

We're going to connect to the Leap Motion via WebSockets, same as usual:

      connection: { name: 'leapmotion', adaptor: 'leapmotion', port: '127.0.0.1:6437' },
      device: { name: 'leapmotion', driver: 'leapmotion' },


For our robot's work, we're going to relate the frame elements, and if we see
any hands, log them, along with the number of fingers currently associated with
each hand.

      work: function(my) {
        my.leapmotion.on('frame', function(frame) {
          frame.addObjectRelations();

          if (frame.anyHands()) {
            console.log("I currently see " + frame.hands.length + " hand(s).");

            for (var i = 0; i < frame.hands.length; i++) {
              var hand = frame.hands[i];
              var fingers = hand.pointables.length;
              console.log("  - Hand #" + hand.id + ": " + fingers + " fingers currently visible");
            }
          }
        });
      }

With that done, let's start the robot up!

    }).start();
