<script lang="ts">
  let pollArray: any[] = []
  let index = 0
  let objectID = 1
  let showPollSetup = false
  let optionOne: string = ''
  let optionTwo: string = ''

  function SetupPoll(votOptionOne: string, voteOptionTwo: string) {
    if(optionOne != '' && optionTwo != '') {
      AddPoll(votOptionOne, voteOptionTwo)
      optionOne = ''
      optionTwo = ''
      showPollSetup = !showPollSetup
    } else {
      alert("Empty fields are not allowed!")
    }
  }

  function AddPoll(voteOptionOne: string, voteOptionTwo: string) {
    pollArray[index] = {id: objectID, voteOptionOne: voteOptionOne, voteOptionTwo: voteOptionTwo, totalVotesFor: 0, totalVotesAgainst: 0}
    index++
    objectID++
  }

  function DeletePoll(PollID: number) {
    pollArray = pollArray.filter((poll) => poll.id != PollID)
    index--
  }
</script>

<div class="w-screen h-screen bg-gray-500">
  {#if showPollSetup}
    <div class="flex justify-center items-center w-full h-full">
      <div class="rounded border-4 p-8">
        <div class="w-full mb-5">
          <input bind:value={optionOne} class="w-full border-2 mb-5" type="text" placeholder="Option 1">

          <input bind:value={optionTwo} class="w-full border-2" type="text" placeholder="Option 2">
        </div>

        <div class="flex">
          <div class="w-full border-2 bg-white hover:bg-green-600 rounded duration-200 mr-4"><button class="w-full" on:click={() => {SetupPoll(optionOne, optionTwo)}}>Create Poll</button></div>

          <div class="w-full border-2 bg-white hover:bg-red-600 rounded duration-200 ml-4"><button class="w-full" on:click={() => {showPollSetup = !showPollSetup}}>Close</button></div>
        </div>
      </div>
    </div>

  {:else}
    <header class="flex justify-center items-center w-full h-[150px] border-b-8">
      <div class="text-center">
        <p class="text-5xl mb-5">Poller</p>
        <button class="w-[100px] bg-green-600 rounded hover:opacity-75 duration-200" on:click={() => {showPollSetup = !showPollSetup}}>Create Poll</button>
      </div>
    </header>
    
    <div class="flex justify-start flex-wrap gap-5 p-8 m-8">
      {#each pollArray as poll (poll.id)}
        <div class="w-[300px] h-[150px] bg-gray-300 border-black border-2 rounded">
          <div class="flex justify-center h-1/3 border-black border-b hover:bg-green-400 duration-150"><button class="w-full" on:click={() => {poll.totalVotesFor++}}>{poll.voteOptionOne} - {poll.totalVotesFor}</button></div>
          <div class="flex justify-center h-1/3 border-black border-b hover:bg-blue-400 duration-150"><button class="w-full" on:click={() => {poll.totalVotesAgainst++}}>{poll.voteOptionTwo} - {poll.totalVotesAgainst}</button></div>
          <div class="flex justify-center h-1/3 hover:bg-red-600 duration-150"><button class="w-full" on:click={() => {DeletePoll(poll.id)}}>Delete</button></div>
        </div>
        {:else}
          <div class="flex justify-center items-center w-screen">No polls at the moment. ¯\_(ツ)_/¯</div>
      {/each}
    </div>
  {/if}
</div>