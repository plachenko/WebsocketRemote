<script>
  import { onMount } from 'svelte';
  import WS from './WS.svelte';
  import Capture from './Capture.svelte';

  let recognition;
  let finalTranscript = '';

  function startRecognition() {
    finalTranscript = ''; // Clear the previous transcript
    recognition.start();
  }

  function stopRecognition() {
    recognition.stop();
  }

  onMount(() => {
    if ('SpeechRecognition' in window || 'webkitSpeechRecognition' in window) {
      // Create the SpeechRecognition object
      recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();

      // Set properties for the SpeechRecognition object
      recognition.interimResults = true;
      recognition.continuous = true;

      // Event listeners for recognition results
      recognition.onresult = event => {
        let interimTranscript = '';
        for (let i = event.resultIndex; i < event.results.length; ++i) {
          const transcript = event.results[i][0].transcript;
          if (event.results[i].isFinal) {
            finalTranscript += transcript;
          } else {
            interimTranscript += transcript;
          }
        }
      };

      recognition.onend = () => {
        // Speech recognition ended
        console.log('Recognition ended');
      };

      recognition.onerror = event => {
        // Handle error
        console.error('Speech recognition error:', event.error);
      };
    } else {
      // SpeechRecognition not supported
      console.error('SpeechRecognition is not supported in this browser.');
    }
  });


  function capturePointer(e){

    if(e.detail.event.pressure){
        startRecognition();
    }else{
        stopRecognition();
    }
  }
</script>

<main>
    <Capture on:pointerdown={capturePointer} />
    <WS />

    <p>
        <span>{finalTranscript}</span>
    </p>

</main>

<style>
     
</style>
