
<button type={props.type} class="relative flex overflow-hidden rounded-xl p-px">
    {#if withShine}
        <span class="absolute inset-[-1000%] animate-[spin_2s_linear_infinite] bg-[conic-gradient(from_90deg_at_50%_50%,#c2c2c2_0%,#505050_50%,#bebebe_100%)]"></span>
    {/if}
    <span class={['flex h-full w-full cursor-pointer items-center justify-center rounded-[11px] transition-colors ease-in-out duration-500', btnClass, 'px-4 py-2 text-sm font-medium backdrop-blur-3xl', props.class ]} >
        {#if props.icon}
            <i class="inline mr-2" data-lucide={props.icon}></i>
        {/if} 
        {props.text}
    </span>
</button>

<script>
    let props = $props();
    let btnClass = $state('bg-neutral-950 text-gray-50');
    let withShine = $state(false);
    withShine = props.withShine;
    if(props.sType == 'success') btnClass = 'bg-emerald-900 text-emerald-400 border border-emerald-900 hover:bg-emerald-800';
</script>