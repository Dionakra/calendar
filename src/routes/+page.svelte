<script lang="ts">
  import { onMount } from "svelte";

  // Importing data from the server
  export let data;
  let selectedSubjects = data.paths.flatMap((path) =>
    path.subjects.map((subject) => subject.id)
  );

  // Local Storage Key
  const LOCAL_STORAGE_KEY = "SELECTED_SUBJECTS";

  function toggleSelectedSubject(subject: string) {
    const index = selectedSubjects.indexOf(subject);

    if (index >= 0) {
      selectedSubjects.splice(index, 1);
    } else {
      selectedSubjects.push(subject);
    }

    // Due to some Svelte quirks, for an array to be updated, it needs a reassignment
    selectedSubjects = selectedSubjects;

    localStorage.setItem(LOCAL_STORAGE_KEY, selectedSubjects.join(","));
  }

  function getMonthName(monthNumber: number): string {
    const date = new Date();
    date.setMonth(monthNumber);

    return date.toLocaleString("es-ES", {
      month: "long",
    });
  }

  onMount(() => {
    const saved = localStorage.getItem(LOCAL_STORAGE_KEY);
    if (saved != null) {
      selectedSubjects = saved.split(",");
    }
  });

  function isToday(day: Date): boolean {
    if (!day) {
      return false;
    }
    return day.toDateString() == new Date().toDateString();
  }
</script>

<svelte:head>
  <title>Calendario Master 2023/2024</title>
  <meta name="description" content="Calendario Master 2023/2024" />
</svelte:head>

<!-- LEGEND -->
<div class="flex flex-wrap mx-auto justify-center">
  <div class="bg-gray-50">
    <!-- LEGEND -->
    <div class="flex flex-wrap mx-auto justify-center">
      {#each data.paths as path}
        <div class="w-1/2 sm:w-1/6 text-xs">
          <div class="mx-0.5">
            <p class="text-sm pl-1">{path.name}</p>
            {#each path.subjects as subject}
              <!-- svelte-ignore a11y-click-events-have-key-events -->
              <!-- svelte-ignore a11y-no-static-element-interactions -->
              <div
                class="my-1 hover:cursor-pointer text-xs px-2 py-1 rounded {selectedSubjects.includes(
                  subject.id
                )
                  ? `bg-${path.color}-200`
                  : 'bg-gray-200'}"
                on:click={(c) => toggleSelectedSubject(subject.id)}
              >
                <span class="font-bold">{subject.id}</span>: {subject.name}
              </div>
            {/each}
          </div>
        </div>
      {/each}
    </div>

    <!-- CALENDAR -->
    <div class="flex flex-wrap">
      {#each data.months as month}
        <div class="w-full md:w-1/2">
          <div class="mx-1 my-4 text-sm">
            <h2 class="text-xl text-center capitalize">
              {getMonthName(month.number)}
            </h2>
            <div class=" rounded-md shadow-md bg-white border border-gray-50">
              <div
                class="capitalize flex text-center text-white py-1 bg-inma rounded-t-md font-bold"
              >
                <div class="w-1/5">Lunes</div>
                <div class="w-1/5">Martes</div>
                <div class="w-1/5">Miercoles</div>
                <div class="w-1/5">Jueves</div>
                <div class="w-1/5">Viernes</div>
              </div>

              <div class="text-sm text-gray-700 border-t border-gray-100">
                {#each month.weeks as week}
                  <div class="w-full">
                    <div class="flex w-full divide-x divide-gray-100">
                      {#each week.days as day}
                        <div class="w-1/5 h-auto">
                          {#if day != undefined}
                            <div
                              class="h-full pb-8"
                              class:bg-gray-100={day.holiday != undefined}
                              class:bg-yellow-100={isToday(day.date)}
                            >
                              <p class="text-right pr-1">
                                {day.date.getDate()}
                              </p>
                              <!-- HOLIDAYS -->
                              {#if day.holiday}
                                <div class="text-center mt-1">
                                  {day.holiday}
                                </div>
                              {/if}

                              <!-- SPECIAL DAYS -->
                              {#if day.special}
                                <div class="text-center mt-1 h-full">
                                  <div
                                    class="rounded-md px-2 py-1 my-2 mx-0.5 h-full bg-inma text-white font-bold"
                                  >
                                    {day.special}
                                  </div>
                                </div>
                              {/if}

                              <!-- EXAMS -->
                              {#each day.lectures as lecture}
                                <div
                                  class:hidden={!selectedSubjects.includes(
                                    lecture.subject.id
                                  )}
                                  class="flex justify-between rounded-md px-2 py-1 my-2 mx-0.5 {`bg-${lecture.color}-200`}"
                                >
                                  <span
                                    >{lecture.subject.id} - {lecture.room}</span
                                  >
                                  {#if lecture.isExam}
                                    <span class="relative flex h-3 w-3 my-auto">
                                      <span
                                        class="{`bg-${lecture.color}-400`} animate-ping absolute inline-flex h-full w-full rounded-full opacity-75"
                                      />
                                      <span
                                        class="{`bg-${lecture.color}-500`} relative inline-flex rounded-full h-3 w-3"
                                      />
                                    </span>
                                  {/if}
                                </div>
                              {/each}
                            </div>
                          {/if}
                        </div>
                      {/each}
                    </div>
                  </div>
                {/each}
              </div>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</div>
