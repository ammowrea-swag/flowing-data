<!--
@component
This is your page!
-->
<script>
  // Import all the news furniture components
  import ArticleHeader from '$lib/components/ArticleHeader.svelte';
  import ArticleBody from '$lib/components/ArticleBody.svelte';
  import Image from '$lib/components/Image.svelte';
  import RelatedLinks from '$lib/components/RelatedLinks.svelte';
  import RestaurantTable from '$lib/components/RestaurantTable.svelte';

  // Article metadata
  let headline = 'Become a force for good. Join our next class.';
  let byline = 'NYCity News Service';
  let pubDate = '2026-01-31';


  // Related stories
  const relatedStories = [
    { headline: 'How America\'s top news organizations escape rigid publishing systems to design beautiful data-driven stories on deadline.', href: 'https://palewi.re/docs/coding-the-news/' },
    { headline: 'How to install, configure and use Visual Studio Code, GitHub and Copilot', href: 'https://palewi.re/docs/coding-the-news/scripts/week-1/' },
    { headline: "How to publish a website with Node.JS and GitHub Actions", href:"https://palewi.re/docs/coding-the-news/scripts/week-2/"},
  ];

  // Data //
    let { data } = $props(); 

    // set boro pulldown filter// 
    let selectedBorough = $state("");
 
  // Get unique cuisines for the filter dropdown
     let selectedCuisine = $state("");

  // Search query filter //
  let searchQuery = $state("");

  // Get unique cuisines for the filter dropdown //
  let cuisines = $derived(
    data?.restaurants 
      ? [...new Set(data.restaurants.map(r => r.cuisine_description))].sort()
      : []
  );

  // Get unique grades for the filter dropdown //
  let grades = $derived(
    data?.restaurants 
      ? [...new Set(data.restaurants.map(r => r.grade).filter(g => g))].sort()
      : []
  );

  // Grade filter //
  let selectedGrade = $state("");

  // Filter restaurants based on selected filters //
  let restaurants = $derived(
    data?.restaurants
      ? data.restaurants.filter(r => {
          if (selectedBorough !== '' && r.boro !== selectedBorough) return false;
          if (selectedCuisine !== '' && r.cuisine_description !== selectedCuisine) return false;
          if (selectedGrade !== '' && r.grade !== selectedGrade) return false;
          if (searchQuery !== '' && !r.dba.toLowerCase().includes(searchQuery.toLowerCase())) return false;
          return true;
        })
      : []
  );

  // Display limited results //
  let displayed = $derived(restaurants.slice(0, 50));

</script>

<!-- This sets the page title in the browser tab -->
<svelte:head>
  <title>{headline} | NYCity News Service</title>
  <meta name="description" content="At the Craig Newmark Graduate School of Journalism at the City University of New York, change is in our DNA. That comes of being born in 2006, as the digital revolution was transforming our profession in ways none of us could have imagined." />
</svelte:head>

<!-- Your page content goes here -->
<div class="container">
  
  <!-- Article Header: Headline, byline, and publication date -->
  <ArticleHeader
    {headline}
    {byline}
    {pubDate}
  />

  <!-- Lead Image: Animated gif of students at the journalism school -->
  <Image
    src="/example-photo.gif"
    alt="The Craig Newmark Graduate School of Journalism is at 219 West 40th Street in Midtown Manhattan."
    caption="The Craig Newmark Graduate School of Journalism is at 219 West 40th Street in Midtown Manhattan."
    credit="Craig Newmark Graduate School of Journalism"
  />

  <!-- Data Visualization: A table of restaurant inspection data -->
<div class="filters">
  <label for="borough">Borough</label>
  <select id="borough" bind:value={selectedBorough}>
    <option value="">All boroughs</option>
    <option value="Manhattan">Manhattan</option>
    <option value="Brooklyn">Brooklyn</option>
    <option value="Queens">Queens</option>
    <option value="Bronx">Bronx</option>
    <option value="Staten Island">Staten Island</option>
  </select>

  <label for="cuisine">Cuisine</label>
  <select id="cuisine" bind:value={selectedCuisine}>
    <option value="">All cuisines</option>
    {#each cuisines as cuisine}
      <option value={cuisine}>{cuisine}</option>
    {/each}
  </select>

   <label for="grade">Grade</label>
  <select id="grade" bind:value={selectedGrade}>
    <option value="">All grades</option>
    {#each grades as grade}
      <option value={grade}>{grade}</option>
    {/each}
  </select>

</div>

  <div>
    <label for="search">Search by name</label>
    <input id="search" type="text" bind:value={searchQuery} placeholder="e.g. Pizza" />
  </div>

<p class="count">Showing {displayed.length} of {restaurants.length} restaurants</p>
<RestaurantTable data={displayed} />


  <!-- Article Body: The main story text with proper typography -->
  <ArticleBody>
    <p>
      At the Craig Newmark Graduate School of Journalism at the City University of New York, change is in our DNA. That comes of being born in 2006, as the digital revolution was transforming our profession in ways none of us could have imagined.
    </p>

    <p>
      We fashioned a school to teach the latest storytelling, entrepreneurial, and technological skills alongside reporting, writing, and ethics. Beyond that, we’ve crafted a culture that spurns complacency, that isn’t afraid to pivot before the ground under us shifts.
    </p>


  </ArticleBody>

  <!-- Related Stories: Links to other articles -->
  <RelatedLinks
    title="Related Stories"
    links={relatedStories}
  />

</div>

<style>
  /* Styles here only apply to this page */
  .container {
    padding: var(--spacing-lg) var(--spacing-md);
  } 
  .filters {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    align-items: flex-end;
    padding: 1rem;
    background: #f8f8f8;
    border: 1px solid #e0e0e0;
    border-radius: 4px;
    margin-bottom: 0.75rem;
  }

  label {
    display: block;
    font-size: 0.75rem;
    font-weight: bold;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    margin-bottom: 0.25rem;
  }

 select {
    display: block;
    padding: 0.375rem 0.5rem;
    font-size: 0.875rem;
    border: 1px solid #ccc;
    border-radius: 3px }

  input[type="text"] {
  display: block;
  padding: 0.375rem 0.5rem;
  font-size: 0.875rem;
  border: 1px solid #ccc;
  border-radius: 3px;
}

  .count {
    font-size: 0.8rem;
    color: #888;
    margin: 0 0 0.5rem;
  }

</style>