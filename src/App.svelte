<script>
  let notes = [];
  let newNote = {
    x: 50,
    y: 50,
    header: 'Sticky Note',
    text: 'Edit me!',
    backgroundColor: 'yellow',
    isDragging: false,
  };

  function addStickyNote() {
    notes = [...notes, { ...newNote }];
  }

  function handleNoteMove(index, event) {
    if (notes[index].isDragging) {
      notes[index].x = event.clientX - notes[index].dragStartX;
      notes[index].y = event.clientY - notes[index].dragStartY;
    }
  }

  function startDragging(index, event) {
    notes[index].isDragging = true;
    notes[index].dragStartX = event.clientX - notes[index].x;
    notes[index].dragStartY = event.clientY - notes[index].y;
  }

  function stopDragging(index) {
    notes[index].isDragging = false;
  }

  function changeNoteColor(index, color) {
    notes[index].backgroundColor = color;
  }

  function changeNoteHeader(index, newHeader) {
    notes[index].header = newHeader;
  }
</script>

<main>
  <h1>Welcome to Sticky Notes</h1>
  <button on:click={addStickyNote}>Add Sticky Note</button>

  <div class="color-buttons">
    <button on:click={() => changeNoteColor(notes.length - 1, 'yellow')}>Yellow</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'pink')}>Pink</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'lightblue')}>Light Blue</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'lightgreen')}>Light Green</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'lightcoral')}>Light Coral</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'lightsalmon')}>Light Salmon</button>
    <button on:click={() => changeNoteColor(notes.length - 1, 'lightpink')}>Light Pink</button>
  </div>

  {#each notes as note, index (note.x)}
    <div
      class="custom-sticky-note"
      style="left: {note.x}px; top: {note.y}px; background-color: {note.backgroundColor};"
      on:mousemove={event => handleNoteMove(index, event)}
      role="button"
      aria-grabbed="{note.isDragging}"
      tabindex="0"
    >
      <div
        class="sticky-header"
        on:mousedown={event => startDragging(index, event)}
        on:mouseup={() => stopDragging(index)}
      >
        <input
          type="text"
          bind:value={note.header}
          placeholder="Sticky Note"
          class="header-input"
          on:input={() => changeNoteHeader(index, note.header)}
        />
      </div>
      <textarea bind:value={note.text}></textarea>
    </div>
  {/each}
</main>

<style>
  .custom-sticky-note {
    position: absolute;
    width: 250px;
    height: 200px;
    border: 2px solid #ccc;
    padding: 0;
    margin: 0;
  }

  .sticky-header {
    background-color: #ccc;
    padding: 8px;
    cursor: move;
  }

  .header-input {
    border: none;
    background: none;
    font-size: 16px;
    font-family: Arial, sans-serif;
  }

  textarea {
    width: 100%;
    height: calc(100% - 40px); /* Leave space for header and adjustments */
    border: none;
    background: none;
    resize: none;
  }


//code version1
  .color-buttons {
    margin-top: 10px;
  }
</style>
