<script>
	import '@evidence-dev/tailwind/fonts.css';
	import '../app.css';
	import { EvidenceDefaultLayout } from '@evidence-dev/core-components';
	export let data;
</script>

<div class="flex flex-col min-h-screen w-full">
    <!-- Main Content Area with Sidebar and Breadcrumbs Hidden -->
    <EvidenceDefaultLayout {data} hideBreadcrumbs hideSidebar class="max-w-[100vw] w-full mx-auto flex-grow">
        <slot slot="content" />
    </EvidenceDefaultLayout>

    <!-- Footer Navigation Bar -->
    <footer class="bg-gray-900 text-white py-4 px-6 shadow-md mt-auto">
        <ul class="flex justify-center gap-6 text-lg">
            <li><a href="/" class="hover:text-green-400">🏠 Home</a></li>
            <li><a href="/cashflow" class="hover:text-green-400">💰 Cashflow</a></li>
            <li><a href="/gg_mmr_bs" class="hover:text-green-400">📊 Balance Sheet</a></li>
            <li><a href="/gg_income_statement" class="hover:text-green-400">💵 Income Statement</a></li>
            <li><a href="/gg_ytd_income" class="hover:text-green-400">🏦 YTD Income Statement</a></li>
            <li><a href="/gg_consolidated" class="hover:text-green-400">💵 Consolidated Income</a></li>
            <li><a href="/gg_plantutilization" class="hover:text-green-400">🏭 PlantUtilization</a></li>
            <li><a href="/gg_comment_business" class="hover:text-green-400">📈 Business Comments</a></li>
        </ul>
    </footer>
</div>
