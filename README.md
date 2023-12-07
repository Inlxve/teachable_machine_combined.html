<html lang="en">

<head>

  <meta charset="UTF-8">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Teachable Machine Image Model</title>

  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest/dist/tf.min.js"></script>

  <script src="https://cdn.jsdelivr.net/npm/@teachablemachine/image@latest/dist/teachablemachine-image.min.js"></script>

</head>

<body>

  <div>Teachable Machine Image Model</div>

  <button type="button" onclick="init()">Start</button>

  <div id="webcam-container"></div>

  <div id="label-container"></div>

  <script type="text/javascript">

    // the link to your model provided by Teachable Machine export panel

    const modelURL = "https://raw.githubusercontent.com/your-username/your-repo/master/my_model/model.json";

    const metadataURL = "https://raw.githubusercontent.com/your-username/your-repo/master/my_model/metadata.json";

    let model, webcam, labelContainer, maxPredictions;

    async function init() {

      // load the model and metadata

      model = await tmImage.load(modelURL, metadataURL);

      maxPredictions = model.getTotalClasses();

      // Convenience function to setup a webcam

      const flip = true; // whether to flip the webcam

      webcam = new tmImage.Webcam(200, 200, flip); // width, height, flip

      await webcam.setup(); // request access to the webcam

      await webcam.play();

      window.requestAnimationFrame(loop);

      // append elements to the DOM

      document.getElementById("webcam-container").appendChild(webcam.canvas);

      labelContainer = document.getElementById("label-container");

      for (let i = 0; i < maxPredictions; i++) { // and class labels

        labelContainer.appendChild(document.createElement("div"));

      }

    }

    async function loop() {

      webcam.update(); // update the webcam frame

      await predict();

      window.requestAnimationFrame(loop);

    }

    // run the webcam image through the image model

    async function predict() {

      // predict can take in an image, video, or canvas html element

      const prediction = await model.predict(webcam.canvas);

      for (let i = 0; i < maxPredictions; i++) {

        const classPrediction =

          prediction[i].className + ": " + prediction[i].probability.toFixed(2);

        labelContainer.childNodes[i].innerHTML = classPrediction;

      }

    }

  </script>

</body>

</html>
