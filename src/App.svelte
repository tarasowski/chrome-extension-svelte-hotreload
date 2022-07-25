<script>
  import logo from './assets/svelte.png'
  import Counter from './lib/Counter.svelte'
  import './lib/Tailwind.css'
  import { onMount } from "svelte"
  let increment;

  console.log("hello world")

  let query = "filter:follows min_faves:20"
  let data = ""

  const encodeHTML = (html) => {
    // Initialize the DOM parser
    var parser = new DOMParser();

    // Parse the text
    var doc = parser.parseFromString(html, "text/html");

    // You can now even select part of that html as you would in the regular DOM 
    // Example:
    // var docArticle = doc.querySelector('article').innerHTML;

    return doc
  }

  const runSearch = () =>
    fetch(`https://twitter.com/search?q=${query}`) 
      .then(e => console.log(e) || e)
      .then(res => res.text())
      .then(res => encodeHTML(res))
      .then(doc => doc.querySelector('[data-testid="tweetText"]'))
      .then(d => console.log(d) || d)
      .then(d => data = d)
      .catch(e => {console.log(e); data = e.message})

</script>

<section class="text-gray-600 body-font mt-20">
  <p>Hotreloading works</p>
      <div class="ml-20">
        <p>{data}</p>
        <button class="inline-flex text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg" on:click={runSearch}>Button</button>
      </div>
</section>
