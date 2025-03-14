<script lang="ts">
  import Doc from '$theme/Doc.svelte'
  import type { SvelteComponentDoc } from 'sveltedoc-parser'
  import Description from './Description.svelte'
  import InlineCode from './InlineCode.svelte'
  import VariableName from './VariableName.svelte'

  export let api: SvelteComponentDoc

  $: propId = `${api.name} Props`
  $: eventId = `${api.name} Events`
  $: slotsId = `${api.name} Slots`
</script>

<Doc title={propId} id="props" hideSandboxIcon pb-2 hideEditIcon>
  {#if api.data?.length}
    <ul m-0 fs-14>
      {#each api.data as { defaultValue, name, type, description }}
        <li mb-4>
          <div flex items-center flex-wrap>
            <VariableName>
              {name}
            </VariableName>
            <div mx-2>
              <InlineCode>
                {type?.text}
              </InlineCode>
            </div>
            <span>default: <InlineCode>{defaultValue}</InlineCode></span>
            <Description>
              {@html description}
            </Description>
          </div>
        </li>
      {/each}
    </ul>
  {:else}
    <div text-t-2 px-4 pb-4 dark:text-8c8c8c>No data</div>
  {/if}
</Doc>

<Doc title={eventId} id="events" hideSandboxIcon pb-2 hideEditIcon>
  {#if api.events?.length}
    <ul m-0 fs-14>
      {#each api.events as { name, description }}
        <li mb-4>
          <VariableName>
            {name}
          </VariableName>
          <Description>
            {@html description}
          </Description>
        </li>
      {/each}
    </ul>
  {:else}
    <div text-t-2 dark:text-8c8c8c px-4 pb-4>No data</div>
  {/if}
</Doc>

<Doc title={slotsId} id="slots" mb-16 hideSandboxIcon pb-2 hideEditIcon>
  {#if api.slots?.length}
    <ul m-0 fs-14>
      {#each api.slots as { name, description, params }}
        <li mb-4>
          <span bg-primary bg-opacity-20 rounded-1 px-2 py-1>{name}</span>
          <Description>
            {@html description}
          </Description>
          {#if params && params.length}
            <p>Bindings:</p>
            <ul>
              {#each params as param}
                <li mb-4>
                  <VariableName>
                    {param.name}
                  </VariableName>
                  <InlineCode>
                    {param.type.text}
                  </InlineCode>
                  <Description>
                    {@html description}
                  </Description>
                </li>
              {/each}
            </ul>
          {/if}
        </li>
      {/each}
    </ul>
  {:else}
    <div text-t-2 px-4 pb-4 dark:text-8c8c8c>No data</div>
  {/if}
</Doc>
