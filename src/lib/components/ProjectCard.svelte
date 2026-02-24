<script lang="ts">
export let link: string = '';
export let buttonText: string = 'Project Description';
export let image: string = 'https://via.placeholder.com/150';
</script>

<a  href="{link}" target="_blank" class="card shadow-xl hover:shadow-2xl bg-surface-50 rounded-lg p-4 flex flex-col gap-4 border-1 border-gray-200 md:max-h-[28rem] dark:bg-surface-900">
    <div class="items-center self-center">
        <img src="{image}" alt="Project" class="w-full h-auto">        
    </div>
    <div class="self-center w-full text-start">
        <p>{buttonText}</p>        
    </div>

</a>