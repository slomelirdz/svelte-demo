<script>
  export let label;
  export let checked = false;

  const uniqueID = Math.floor(Math.random() * 100);

  const toggle = (event) => {
      checked = event.target.getAttribute('aria-checked') === 'true' ? false : true;
  }
</script>

<div class="toggle-switch">
  <span id={`switch-${uniqueID}`}>{label}</span>
  <button
      role="switch"
      aria-checked={checked}
      aria-labelledby={`switch-${uniqueID}`}
      on:click={toggle}>
  </button>
</div>

<style lang="scss">
  .toggle-switch {
      display: flex;
      align-items: center;

      button {
        background: var(--gray);
        border-radius: 1.5em;
        border: none;
        cursor: pointer;
        height: 1.6em;
        margin: 0 0 0 0.5em;
        position: relative;
        width: 3em;

        &:before {
          background: #fff;
          border-radius: 100%;
          content: '';
          height: 1.3em;
          position: absolute;
          right: 1.5em;
          top: 0.15em;
          transition: transform 0.3s;
          width: 1.3em;
        }

        &:focus {
          border-radius: 1.5em;
        }

        &[aria-checked='true']{
          background-color: var(--primary-color);

          &:focus {
            box-shadow: 0 0px 8px var(--primary-color);
          }

          &::before {
            transform: translateX(1.3em);
            transition: transform 0.3s;
          }
        }
    }
  }
</style>
