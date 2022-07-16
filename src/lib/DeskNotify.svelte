<script lang="ts">
  import { onMount } from "svelte";

  const options = {
    body: "This is the body of the Notification",
    icon: "https://images.pexels.com/photos/853168/pexels-photo-853168.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500",
    //   dir: "ltr"
  };

  onMount(() => {});

  //   function notifyMe() {
  //     if (!("Notification" in window)) {
  //       console.log("This browser does not support desktop notification");
  //     } else {
  //       Notification.requestPermission();
  //     }

  //     Notification.requestPermission(function (result) {
  //       if (result === "granted") {
  //         navigator.serviceWorker.ready.then(function (registration) {
  //           registration.showNotification("Vibration Sample", {
  //             body: "Buzz! Buzz!",
  //             icon: "../images/touch/chrome-touch-icon-192x192.png",
  //             vibrate: [200, 100, 200, 100, 200, 100, 200],
  //             tag: "vibration-sample",
  //           });
  //         });
  //       }
  //     });
  //   }

  function notifyMe() {
    // Let's check if the browser supports notifications
    if (!("Notification" in window)) {
      alert("This browser does not support desktop notification");
    }

    // Let's check whether notification permissions have already been granted
    else if (Notification.permission === "granted") {
      // If it's okay let's create a notification
      //   let notification = new Notification("Hi there!", options);
      //   only works in https (non local)
      navigator.serviceWorker.ready.then(function (registration) {
        registration.showNotification("Vibration Sample", {
          body: "Buzz! Buzz!",
          icon: "../images/touch/chrome-touch-icon-192x192.png",
          vibrate: [200, 100, 200, 100, 200, 100, 200],
          tag: "vibration-sample",
        });
      });
    }

    // Otherwise, we need to ask the user for permission
    else if (Notification.permission !== "denied") {
      Notification.requestPermission().then(function (permission) {
        // If the user accepts, let's create a notification
        if (permission === "granted") {
          //   let notification = new Notification("Hi there!", options);
          //   only works in https (non local)
          navigator.serviceWorker.ready.then(function (registration) {
            registration.showNotification("Vibration Sample", {
              body: "Buzz! Buzz!",
              icon: "../images/touch/chrome-touch-icon-192x192.png",
              vibrate: [200, 100, 200, 100, 200, 100, 200],
              tag: "vibration-sample",
            });
          });
        }
      });
    }

    // At last, if the user has denied notifications, and you
    // want to be respectful there is no need to bother them anymore.
  }
</script>

<button on:click={notifyMe}>Notify me!</button>
