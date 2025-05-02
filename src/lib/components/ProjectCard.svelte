<script lang="ts">
export let link: string = '';
export let buttonText: string = 'Project Description';
export let image: string = 'https://via.placeholder.com/150';
</script>

<a  href="{link}" target="_blank" class="card shadow-xl hover:shadow-2xl bg-surface-50 rounded-lg p-4 flex flex-col items-center gap-4 border-1 border-gray-200">
    <div>
        <img src="{image}" alt="Project">        
    </div>
    <button>{buttonText}</button>
</a>