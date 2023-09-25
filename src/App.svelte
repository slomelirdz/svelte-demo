<script>
  import { onMount } from "svelte";
  import autoAnimate from "@formkit/auto-animate"
  import { IconMoon, IconX, IconTrashX, IconChevronUp, IconChevronDown, IconBrandSvelte  } from '@tabler/icons-svelte';
  import ToggleSwitch from './lib/components/ToggleSwitch.svelte';

  let isDarkTheme = false;
  let tags = ['Autum', 'Winter', 'Spring'];
  let comments = [];
  let primaryColor = isDarkTheme ? '#8c57e3' : '#213547';

  onMount(async() => {
    // Checks if the user's preferred color scheme for their operating system is set to dark mode
    isDarkTheme = window.matchMedia("(prefers-color-scheme: dark)").matches;

    // Fetch list data
    fetchData();
  });

  const handleThemeMode = () => {
      document.documentElement.dataset.theme = isDarkTheme ? 'dark' : 'light';
      primaryColor = isDarkTheme ? '#8c57e3' : '#213547';
      // TODO: Store this value to maintain this user's preference
      // localStorage.setItem('siteTheme', theme);
  };

  const fetchData = async () => {
    // Fetch the data from the API
    await fetch('https://jsonplaceholder.typicode.com/post/1/comments')
    .then(response => response.json())
    .then(json => {
      comments = json;
    });
  };

  const removeComment = (target) => {
    comments = comments.filter((comment) => target !== comment);
  }

  const addTag = (e) => {
    if (e.which === 13) {
      tags.push(e.target.value);
      tags = tags;
      e.target.value = '';
    }
  }

  const removeTag = (target) => {
    tags = tags.filter((tag) => target !== tag);
  }

  const moveUp = (index) => {
    if (index) {
      comments.splice(index - 1, 0, ...comments.splice(index, 1));
      comments = comments;
    }
  }

  const moveDown = (index) => {
    if (index < comments.length - 1) {
      comments.splice(index + 1, 0, ...comments.splice(index, 1));
      comments = comments;
    }
  }

  // Sets isDarkTheme as a reactive variable
  $: isDarkTheme, handleThemeMode();
</script>

<main>
  <div class="card">
    <h1 class="title">Svelte Demo <IconBrandSvelte size={50}/></h1>
    <p>Created by <a href="https://www.sandralomeli.com" target="_blank">Sandra Lomeli</a>.</p>
    <code>
      Hi! This is my first project in Svelte. I created it using Vite, choosing Svelte and JavaScript as my stack.
      <br/>Other resources and packages I've used include:
      <br/>
      <br/>- Sass as the css processor.
      <br/>- JSONplaceholder, endpoint to simulate the behavior of a real API (<a href="https://jsonplaceholder.typicode.com/" target="_blank">https://jsonplaceholder.typicode.com/</a>).
      <br/>- AutoAnimate for transition animations (<a href="https://auto-animate.formkit.com/" target="_blank">https://auto-animate.formkit.com/</a>).
      <br/>- TablerICONS for iconography (<a href="https://tabler-icons.io/" target="_blank">https://tabler-icons.io/</a>).
      <br/>- Css-loaders for loader animation (<a href="https://css-loaders.com/" target="_blank">https://css-loaders.com/</a>).
      <br/>- Installed the Svelte extension for VS Code.
    </code>
  </div>
  <!-- Toggle Switch -->
  <div class="card">
    <h2>Toogle Switch</h2>
    <div class="toggle">
      <IconMoon size={20} class={'icon-moon'} color={isDarkTheme ? primaryColor : 'currentCOlor'}/>
      <ToggleSwitch bind:checked={isDarkTheme} label={'Dark Mode'}/>
    </div>
  </div>
  <!-- Tags -->
  <div class="card">
    <h2>Tags</h2>
    <p>Type any word and press 'Enter' to create a new tag.</p>
    <input
      class="input"
      id="add-tag-input"
      type="text"
      placeholder="Add a tag..."
      on:keydown={addTag}
    />
    <ul class="tags" use:autoAnimate>
      {#each tags as tag}
        <li class="item">
          <span>{tag}</span>
          <button class="button-icon" on:click={() => removeTag(tag)}>
            <IconX size={14}/>
          </button>
        </li>
      {/each}
    </ul>
  </div>
  <!-- List Rendering -->
  <div class="card">
    <h2>List Rendering</h2>
    <p>Remove items from the list and rearrange them by moving them up and down.</p>
    <ul class="comments" use:autoAnimate>
      {#each comments as comment, index}
        <li class="item">
          <span class="name">{comment.name}</span>
          <br/>
          <span class="email">{comment.email}</span>
          <p class="body">{comment.body}</p>
          <div class="controls">
            <button class="button-icon" on:click={() => moveUp(index)}>
              <IconChevronUp color={primaryColor}/>
            </button>
            <button class="button-icon" on:click={() => moveDown(index)}>
              <IconChevronDown color={primaryColor}/>
            </button>
            <button class="button-icon" on:click={() => removeComment(comment)}>
              <IconTrashX color={primaryColor}/>
            </button>
          </div>
        </li>
      {:else}
        <div class="loader"></div>
      {/each}
    </ul>
  </div>
</main>

<style lang="scss">
  .title {
    align-items: center;
    background-clip: text;
    background-image: linear-gradient(-45deg,var(--gradient-from),var(--gradient-to));
    color: var(--primary-color);
    display: flex;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .toggle {
    display: flex;
    align-items: center;
  }

  .button-icon {
    background-color: transparent;
    border: 0;
    cursor: pointer;
    display: flex;

    &:hover,
    &:active {
      opacity: .8;
    }
  }

  .tags,
  .comments {
    list-style-type: none;
    padding: 0;
    position: relative;
  }

  .tags {
    display: flex;
    flex-wrap: wrap;

    .item {
      align-items: center;
      background-image: linear-gradient(-45deg,var(--gradient-from),var(--gradient-to));
      border-radius: 1.5em;
      color: #fff;
      display: flex;
      margin-bottom: 1em;
      padding: 0 .75em;

      .button-icon {
        padding: 0 0 0 0.5em;
      }

      &:not(last-child) {
        margin-right: 1em;
      }
    }
  }

  .comments {
    display: grid;
    row-gap: 1em;

    .item {
      background-color: var(--bg-gray-color);
      border-radius: .5em;
      padding: 1em;
      text-align: left;

      .name {
        font-weight: 800;
        color: var(--primary-color);
        text-transform: capitalize;
      }

      .email {
        text-transform: lowercase;
      }

      .body {
        text-transform: capitalize;
      }

      .controls {
        display: flex;
        justify-content: end;
      }
    }

  .loader {
    width: 50px;
    aspect-ratio: 1;
    border-radius: 50%;
    border: 8px solid var(--gray);
    border-right-color: var(--primary-color);
    animation: spinner 1s infinite linear;
  }
  @keyframes spinner {
    to{transform: rotate(1turn)}}
  }
</style>
