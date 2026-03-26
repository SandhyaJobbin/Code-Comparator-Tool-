/**

 * Serves the HTML file as a Web App.

 */

function doGet(e) {

  // Use createTemplateFromFile instead of createHtmlOutputFromFile

  // evaluate() is necessary to process the HTML, CSS, and JS properly in GAS

  return HtmlService.createTemplateFromFile('index')

      .evaluate()

      .setTitle('Smart Version Analyzer')

      .addMetaTag('viewport', 'width=device-width, initial-scale=1.0')

      .setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL);

}

 

/**

 * Optional: Include a function if you ever want to split CSS/JS into separate files

 * and include them directly into index.html using <?!= include('filename'); ?>

 */

function include(filename) {

  return HtmlService.createHtmlOutputFromFile(filename).getContent();

}

 

 

 

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Smart Code & Content Comparator</title>

    <!-- Sirf icons ke liye external script hai, baaki sab internal hai -->

    <script src="https://unpkg.com/lucide@latest"></script>

    <style>

        /* =========================================

           INTERNAL CSS (NO TAILWIND REQUIRED)

           ========================================= */

        :root {

            --primary: #2563eb;

            --primary-hover: #1d4ed8;

            --bg-color: #f9fafb;

            --text-main: #111827;

            --text-muted: #6b7280;

            --border-color: #e5e7eb;

            --card-bg: #ffffff;

           

            --success-bg: #ecfdf5;

            --success-text: #047857;

            --danger-bg: #fef2f2;

            --danger-text: #b91c1c;

            --warning-bg: #fefce8;

            --warning-text: #a16207;

 

            /* Editor Dark Theme */

            --editor-bg: #1e1e1e;

            --editor-header: #2d2d2d;

            --editor-border: #3f3f46;

            --editor-text: #d4d4d4;

           

            /* Diff Colors */

            --diff-add-bg: rgba(46, 160, 67, 0.2);

            --diff-add-text: #a6f3a6;

            --diff-rem-bg: rgba(248, 81, 73, 0.2);

            --diff-rem-text: #ffb3b3;

            --diff-empty: rgba(136, 136, 136, 0.1);

        }

 

        * { box-sizing: border-box; margin: 0; padding: 0; }

       

        body {

            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;

            background-color: var(--bg-color);

            color: var(--text-main);

            line-height: 1.5;

        }

 

        /* Layout */

        .navbar {

            background-color: var(--primary);

            color: white;

            padding: 1rem 0;

            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);

            margin-bottom: 2rem;

        }

        .container {

            max-width: 1280px;

            margin: 0 auto;

            padding: 0 1rem;

        }

        .nav-content {

            display: flex;

            align-items: center;

            gap: 1rem;

        }

        .nav-title { font-size: 1.5rem; font-weight: 700; margin-bottom: 0.2rem; }

        .nav-subtitle { font-size: 0.875rem; color: #dbeafe; }

 

        .grid-2 { display: grid; grid-template-columns: 1fr; gap: 1.5rem; margin-bottom: 1.5rem; }

        .grid-3 { display: grid; grid-template-columns: 1fr; gap: 1.5rem; margin-bottom: 1.5rem; }

       

        @media (min-width: 1024px) {

            .grid-2 { grid-template-columns: 1fr 1fr; }

            .grid-3 { grid-template-columns: 2fr 1fr; }

        }

 

        /* Typography & Utilities */

        h3 { font-size: 1.125rem; font-weight: 600; margin-bottom: 1rem; color: var(--text-main); }

        .flex { display: flex; }

        .items-center { align-items: center; }

        .justify-between { justify-content: space-between; }

        .gap-2 { gap: 0.5rem; }

        .gap-4 { gap: 1rem; }

        .hidden { display: none !important; }

        .text-center { text-align: center; }

        .font-bold { font-weight: 700; }

        .text-sm { font-size: 0.875rem; }

        .text-muted { color: var(--text-muted); }

 

        /* Cards */

        .card {

            background: var(--card-bg);

            border: 1px solid var(--border-color);

            border-radius: 0.75rem;

            padding: 1.5rem;

            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1);

        }

        .card-title {

            font-size: 0.875rem;

            text-transform: uppercase;

            letter-spacing: 0.05em;

            color: var(--text-muted);

            border-bottom: 1px solid var(--border-color);

            padding-bottom: 0.5rem;

            margin-bottom: 1rem;

        }

 

        /* Stats Blocks */

        .stats-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }

        .stat-box {

            padding: 0.75rem;

            border-radius: 0.5rem;

            border: 1px solid var(--border-color);

            background: var(--bg-color);

        }

        .stat-box.added { background: var(--success-bg); border-color: #a7f3d0; color: var(--success-text); }

        .stat-box.removed { background: var(--danger-bg); border-color: #fecaca; color: var(--danger-text); }

        .stat-value { font-size: 1.5rem; font-weight: 700; margin-top: 0.25rem; }

 

        /* Smart Summary */

        .summary-card { background: #eff6ff; border-color: #bfdbfe; }

        .summary-list { margin-left: 1.5rem; font-size: 1rem; color: #1e40af; }

        .summary-list li { margin-bottom: 0.5rem; }

 

        /* Lists */

        .obs-list { list-style: none; }

        .obs-list li {

            display: flex;

            align-items: flex-start;

            gap: 0.75rem;

            padding: 0.75rem;

            background: var(--bg-color);

            border-radius: 0.5rem;

            margin-bottom: 0.5rem;

            font-size: 0.95rem;

        }

 

        /* Button */

        .btn-analyze {

            background-color: var(--primary);

            color: white;

            border: none;

            padding: 0.75rem 2rem;

            font-size: 1rem;

            font-weight: 600;

            border-radius: 0.5rem;

            cursor: pointer;

            display: inline-flex;

            align-items: center;

            gap: 0.5rem;

            box-shadow: 0 4px 6px -1px rgba(37, 99, 235, 0.3);

            transition: all 0.2s;

            margin-bottom: 2rem;

        }

        .btn-analyze:hover { background-color: var(--primary-hover); transform: translateY(-2px); }

 

        /* Progress Bars */

        .progress-container { margin-bottom: 1.25rem; }

        .progress-header { display: flex; justify-content: space-between; font-size: 0.875rem; margin-bottom: 0.25rem; }

        .progress-track { width: 100%; height: 0.5rem; background-color: var(--border-color); border-radius: 1rem; overflow: hidden; }

        .progress-fill { height: 100%; transition: width 1s ease-in-out; }

        .fill-prev { background-color: #9ca3af; }

        .fill-curr { background-color: var(--primary); }

 

        /* =========================================

           EDITORS & DIFF VIEW (VS CODE STYLE)

           ========================================= */

        .editor-wrapper { display: flex; flex-direction: column; gap: 0.5rem; }

        .editor-label { font-weight: 600; display: flex; align-items: center; gap: 0.5rem; color: #374151; }

       

        .editor-container {

            display: flex;

            background-color: var(--editor-bg);

            border: 1px solid var(--editor-border);

            border-radius: 0.5rem;

            height: 400px;

            overflow: hidden;

        }

        .line-numbers {

            padding: 1rem 0.5rem;

            background-color: var(--editor-bg);

            color: #858585;

            text-align: right;

            font-family: monospace;

            font-size: 0.875rem;

            line-height: 1.5rem;

            user-select: none;

            border-right: 1px solid var(--editor-border);

            min-width: 3rem;

            overflow: hidden;

        }

        .code-textarea {

            flex: 1;

            padding: 1rem;

            background-color: transparent;

            color: var(--editor-text);

            font-family: monospace;

            font-size: 0.875rem;

            line-height: 1.5rem;

            border: none;

            resize: none;

            outline: none;

            white-space: pre;

            overflow: auto;

        }

 

        /* Diff Container */

        .diff-wrapper {

            background: var(--editor-bg);

            border-radius: 0.75rem;

            border: 1px solid var(--editor-border);

            overflow: hidden;

            display: flex;

            flex-direction: column;

            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.5);

        }

        .diff-header {

            background: var(--editor-header);

            padding: 0.75rem 1rem;

            display: flex;

            justify-content: space-between;

            align-items: center;

            border-bottom: 1px solid var(--editor-border);

            color: white;

            font-weight: 600;

        }

        .diff-legend { display: flex; gap: 1rem; font-size: 0.75rem; font-weight: normal; }

        .legend-item { display: flex; align-items: center; gap: 0.25rem; color: #d1d5db; }

        .legend-color { width: 12px; height: 12px; border-radius: 2px; }

        .color-add { background: #2ea043; }

        .color-rem { background: #f85149; }

 

        .diff-cols-header {

            display: flex;

            background: var(--editor-header);

            border-bottom: 1px solid var(--editor-border);

            font-size: 0.75rem;

            text-transform: uppercase;

            font-weight: 600;

            letter-spacing: 0.05em;

        }

        .diff-col-title { flex: 1; text-align: center; padding: 0.5rem; }

        .diff-col-title.prev { color: #9ca3af; border-right: 1px solid var(--editor-border); }

        .diff-col-title.curr { color: #60a5fa; }

 

        .diff-content-area {

            max-height: 600px;

            overflow-y: auto;

            font-family: monospace;

            font-size: 0.875rem;

            color: var(--editor-text);

        }

 

        /* Unified Diff Row Styling */

        .diff-row {

            display: flex;

            width: 100%;

            min-height: 1.5rem;

            border-bottom: 1px solid rgba(63, 63, 70, 0.5);

        }

        .diff-row:hover { background-color: rgba(255, 255, 255, 0.05); }

       

        .diff-half {

            width: 50%;

            display: flex;

            border-right: 1px solid var(--editor-border);

        }

        .diff-half:last-child { border-right: none; }

       

        .d-num {

            width: 3rem;

            flex-shrink: 0;

            text-align: right;

            padding: 0.1rem 0.5rem;

            color: #6b7280;

            user-select: none;

            border-right: 1px solid var(--editor-border);

        }

        .d-text {

            flex-grow: 1;

            padding: 0.1rem 0.5rem;

            white-space: pre-wrap;

            word-break: break-all;

        }

 

        /* Highlight Classes */

        .h-added-num { background: rgba(46, 160, 67, 0.1); color: var(--diff-add-text); border-left: 2px solid #2ea043; }

        .h-added-txt { background: var(--diff-add-bg); color: var(--diff-add-text); }

       

        .h-rem-num { background: rgba(248, 81, 73, 0.1); color: var(--diff-rem-text); border-left: 2px solid #f85149; }

        .h-rem-txt { background: var(--diff-rem-bg); color: var(--diff-rem-text); text-decoration: line-through; text-decoration-color: rgba(248, 81, 73, 0.5); }

       

        .h-empty-num { background: var(--diff-empty); border-left: 2px solid transparent; }

        .h-empty-txt { background: var(--diff-empty); }

       

        .h-norm-num { background: var(--editor-bg); border-left: 2px solid transparent; }

 

        /* Scrollbars */

        ::-webkit-scrollbar { width: 8px; height: 8px; }

        ::-webkit-scrollbar-track { background: var(--editor-bg); }

        ::-webkit-scrollbar-thumb { background: #424242; border-radius: 4px; }

        ::-webkit-scrollbar-thumb:hover { background: #4f4f4f; }

 

        /* Loader */

        .loader-container { text-align: center; padding: 3rem 0; }

        .spinner {

            border: 4px solid rgba(0,0,0,0.1);

            width: 40px; height: 40px;

            border-radius: 50%;

            border-left-color: var(--primary);

            animation: spin 1s linear infinite;

            margin: 0 auto 1rem;

        }

        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }

    </style>

</head>

<body>

 

    <!-- Navigation Bar -->

    <nav class="navbar">

        <div class="container nav-content">

            <i data-lucide="file-diff" style="width: 32px; height: 32px;"></i>

            <div>

                <h1 class="nav-title">Smart Version Analyzer</h1>

                <p class="nav-subtitle">Compare HTML, Rich Text, or Code with intelligent insights.</p>

            </div>

        </div>

    </nav>

 

    <div class="container">

        <!-- Input Section -->

        <div class="grid-2">

            <!-- Previous Version -->

            <div class="editor-wrapper">

                <label class="editor-label">

                    <i data-lucide="history" style="width: 18px; height: 18px; color: #6b7280;"></i> Previous Version

                </label>

                <div class="editor-container">

                    <div class="line-numbers" id="prev-lines">1</div>

                    <textarea id="prev-code" class="code-textarea" spellcheck="false" onscroll="syncScroll('prev')" oninput="updateLines('prev')"></textarea>

                </div>

            </div>

 

            <!-- Current Version -->

            <div class="editor-wrapper">

                <label class="editor-label">

                    <i data-lucide="file-code-2" style="width: 18px; height: 18px; color: var(--primary);"></i> Current Version

                </label>

                <div class="editor-container">

                    <div class="line-numbers" id="curr-lines">1</div>

                    <textarea id="curr-code" class="code-textarea" spellcheck="false" onscroll="syncScroll('curr')" oninput="updateLines('curr')"></textarea>

                </div>

            </div>

        </div>

 

        <!-- Action Button -->

        <div style="text-align: center;">

            <button onclick="analyzeChanges()" class="btn-analyze">

                <i data-lucide="sparkles" style="width: 20px; height: 20px;"></i> Analyze Changes

            </button>

        </div>

 

        <!-- Loading State -->

        <div id="loading" class="loader-container hidden">

            <div class="spinner"></div>

            <p style="color: var(--text-muted); font-weight: 500;">Analyzing differences and extracting insights...</p>

        </div>

 

        <!-- Results Section (Hidden by default) -->

        <div id="results" class="hidden">

           

            <!-- Top Stats & Summary Grid -->

            <div class="grid-3">

                <!-- Smart Summary -->

                <div class="card summary-card">

                    <h3 style="color: var(--primary); display: flex; align-items: center; gap: 0.5rem; border-bottom: none; font-size: 1rem; text-transform: uppercase;">

                        <i data-lucide="brain-circuit" style="width: 18px; height: 18px;"></i> Smart Summary

                    </h3>

                    <ul id="smart-summary-list" class="summary-list">

                        <!-- Populated by JS -->

                    </ul>

                </div>

 

                <!-- Word Stats -->

                <div class="card">

                    <div class="card-title">Content Stats (Text Only)</div>

                    <div class="stats-grid">

                        <div class="stat-box">

                            <div class="text-sm text-muted">Previous Words</div>

                            <div class="stat-value" id="stat-prev-words">0</div>

                        </div>

                        <div class="stat-box" style="border-color: #bfdbfe;">

                            <div class="text-sm" style="color: var(--primary);">Current Words</div>

                            <div class="stat-value" style="color: var(--primary);" id="stat-curr-words">0</div>

                        </div>

                        <div class="stat-box added">

                            <div class="text-sm">Words Added</div>

                            <div class="stat-value" id="stat-added">+0</div>

                        </div>

                        <div class="stat-box removed">

                            <div class="text-sm">Words Removed</div>

                            <div class="stat-value" id="stat-removed">-0</div>

                        </div>

                    </div>

                </div>

            </div>

 

            <!-- Observations & Metadata Grid -->

            <div class="grid-3" style="grid-template-columns: 2fr 1fr;">

                <!-- Key Observations -->

                <div class="card">

                    <div class="card-title">Key Observations</div>

                    <ul id="observations-list" class="obs-list">

                        <!-- Populated by JS -->

                    </ul>

                </div>

 

                <!-- Metadata & Scoring -->

                <div>

                    <!-- Scoring -->

                    <div class="card" style="margin-bottom: 1.5rem;">

                        <div class="card-title">Quality Score</div>

                       

                        <div class="progress-container">

                            <div class="progress-header">

                                <span class="text-muted">Previous Version:</span>

                                <span class="font-bold text-muted" id="score-prev">0/10</span>

                            </div>

                            <div class="progress-track">

                                <div id="score-bar-prev" class="progress-fill fill-prev" style="width: 0%"></div>

                            </div>

                        </div>

                       

                        <div class="progress-container">

                            <div class="progress-header">

                                <span style="font-weight: 600;">Current Version:</span>

                                <span class="font-bold" style="color: var(--primary); font-size: 1.1rem;" id="score-curr">0/10</span>

                            </div>

                            <div class="progress-track" style="background-color: #dbeafe;">

                                <div id="score-bar-curr" class="progress-fill fill-curr" style="width: 0%"></div>

                            </div>

                        </div>

 

                        <div style="background: var(--warning-bg); border: 1px solid #fef08a; padding: 0.75rem; border-radius: 0.5rem; margin-top: 1rem;">

                            <div style="font-size: 0.75rem; font-weight: 700; color: var(--warning-text); text-transform: uppercase; margin-bottom: 0.25rem;">Recommendation</div>

                            <div style="font-size: 0.875rem; color: #854d0e;" id="score-recommendation"></div>

                        </div>

                    </div>

 

                    <!-- Metadata -->

                    <div class="card">

                        <div class="card-title">Extracted Metadata</div>

                        <div class="flex justify-between" style="font-size: 0.875rem; margin-bottom: 0.5rem;">

                            <span class="text-muted">Author:</span>

                            <span class="font-bold" id="meta-author">Not available</span>

                        </div>

                        <div class="flex justify-between" style="font-size: 0.875rem;">

                            <span class="text-muted">Last Modified:</span>

                            <span class="font-bold" id="meta-modified">Not available</span>

                        </div>

                    </div>

                </div>

            </div>

 

            <!-- Detailed Difference View (Custom CSS Implementation) -->

            <div class="diff-wrapper">

                <!-- Header -->

                <div class="diff-header">

                    <div class="flex items-center gap-2">

                        <i data-lucide="split-square-horizontal" style="width: 16px; height: 16px; color: #9ca3af;"></i> Detailed Difference View

                    </div>

                    <div class="diff-legend">

                        <div class="legend-item"><div class="legend-color color-add"></div> Additions</div>

                        <div class="legend-item"><div class="legend-color color-rem"></div> Deletions</div>

                    </div>

                </div>

               

                <!-- Column Labels -->

                <div class="diff-cols-header">

                    <div class="diff-col-title prev">Previous Version</div>

                    <div class="diff-col-title curr">Current Version</div>

                </div>

               

                <!-- Scrollable Unified Diff Content -->

                <div class="diff-content-area" id="diff-unified-container">

                    <!-- Rows injected here by JavaScript -->

                </div>

            </div>

           

            <div style="height: 2rem;"></div> <!-- Bottom Spacing -->

        </div>

    </div>

 

    <script>

        // Initialize Icons

        lucide.createIcons();

 

        // --- Editor Line Number Logic ---

        function updateLines(editorId) {

            const textarea = document.getElementById(`${editorId}-code`);

            const linesContainer = document.getElementById(`${editorId}-lines`);

            const lineCount = textarea.value.split('\n').length || 1;

           

            let linesHtml = '';

            for (let i = 1; i <= lineCount; i++) {

                linesHtml += i + '<br>';

            }

            linesContainer.innerHTML = linesHtml;

        }

 

        function syncScroll(editorId) {

            const textarea = document.getElementById(`${editorId}-code`);

            const linesContainer = document.getElementById(`${editorId}-lines`);

            linesContainer.scrollTop = textarea.scrollTop;

        }

 

        updateLines('prev');

        updateLines('curr');

 

        // --- Core Analysis Logic ---

 

        function escapeHtml(unsafe) {

            return unsafe

                 .replace(/&/g, "&amp;")

                 .replace(/</g, "&lt;")

                 .replace(/>/g, "&gt;")

                 .replace(/"/g, "&quot;")

                 .replace(/'/g, "&#039;");

        }

 

        function stripHtmlTags(htmlStr) {

            const doc = new DOMParser().parseFromString(htmlStr, 'text/html');

            return doc.body.textContent || "";

        }

 

        function getWordCount(text) {

            const words = text.trim().split(/\s+/);

            return words[0] === "" ? 0 : words.length;

        }

 

        function extractMetadata(htmlStr) {

            const doc = new DOMParser().parseFromString(htmlStr, 'text/html');

            let author = "Not available";

            let modifiedBy = "Not available";

           

            const authorMeta = doc.querySelector('meta[name="author"]');

            if (authorMeta) author = authorMeta.getAttribute('content');

           

            const generatorMeta = doc.querySelector('meta[name="generator"]');

            if (generatorMeta) modifiedBy = generatorMeta.getAttribute('content');

 

            return { author, modifiedBy };

        }

 

        const regexCurrency = /[\$\£\€\₹]|\b(USD|EUR|INR|GBP|Rs\.?)\b/gi;

        const regexDate = /\b\d{1,2}[\/\-]\d{1,2}[\/\-]\d{2,4}\b|\b(?:Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)[a-z]*\s+\d{1,2}(?:st|nd|rd|th)?,\s+\d{4}\b/gi;

        const regexImage = /<img[^>]+>/gi;

 

        function analyzeObservations(prevHTML, currHTML) {

            const observations = [];

           

            const prevPCount = (prevHTML.match(/<p>/gi) || []).length;

            const currPCount = (currHTML.match(/<p>/gi) || []).length;

            if (currPCount > prevPCount) {

                observations.push({ icon: 'align-left', color: '#10b981', text: `Added ${currPCount - prevPCount} new paragraph(s), expanding the content structure.` });

            } else if (prevPCount > currPCount) {

                observations.push({ icon: 'align-left', color: '#ef4444', text: `Removed ${prevPCount - currPCount} paragraph(s), condensing the structure.` });

            }

 

            const prevCurrencies = prevHTML.match(regexCurrency) || [];

            const currCurrencies = currHTML.match(regexCurrency) || [];

            if (currCurrencies.length > prevCurrencies.length) {

                observations.push({ icon: 'coins', color: '#d97706', text: 'New pricing or currency information was added to the document.' });

            } else if (prevCurrencies.length > currCurrencies.length) {

                observations.push({ icon: 'coins', color: '#d97706', text: 'Pricing or currency information was removed or altered.' });

            }

 

            const prevDates = prevHTML.match(regexDate) || [];

            const currDates = currHTML.match(regexDate) || [];

            if (currDates.length > prevDates.length) {

                observations.push({ icon: 'calendar', color: '#3b82f6', text: 'New dates or timeline information have been inserted.' });

            }

 

            const prevImg = (prevHTML.match(regexImage) || []).length;

            const currImg = (currHTML.match(regexImage) || []).length;

            if (currImg !== prevImg) {

                observations.push({ icon: 'image', color: '#8b5cf6', text: `Image count changed from ${prevImg} to ${currImg}.` });

            }

 

            if (observations.length === 0) {

                observations.push({ icon: 'file-text', color: '#6b7280', text: 'Modifications are primarily textual or minor formatting updates.' });

            }

 

            return observations;

        }

 

        function computeLineDiff(prevLines, currLines) {

            const diffLeft = [];

            const diffRight = [];

           

            let i = 0, j = 0;

            let leftLineNum = 1, rightLineNum = 1;

            const maxLookahead = 50;

 

            while (i < prevLines.length || j < currLines.length) {

                const pLine = prevLines[i] !== undefined ? prevLines[i] : null;

                const cLine = currLines[j] !== undefined ? currLines[j] : null;

 

                if (pLine === cLine && pLine !== null) {

                    diffLeft.push({ type: 'unchanged', text: escapeHtml(pLine), line: leftLineNum++ });

                    diffRight.push({ type: 'unchanged', text: escapeHtml(cLine), line: rightLineNum++ });

                    i++; j++;

                } else {

                    let addedOffset = -1;

                    for(let k = 1; k < maxLookahead && (j+k) < currLines.length; k++) {

                        if (pLine === currLines[j+k]) { addedOffset = k; break; }

                    }

                   

                    let removedOffset = -1;

                    for(let k = 1; k < maxLookahead && (i+k) < prevLines.length; k++) {

                        if (cLine === prevLines[i+k]) { removedOffset = k; break; }

                    }

 

                    if (addedOffset !== -1 && (removedOffset === -1 || addedOffset <= removedOffset)) {

                        for(let k=0; k<addedOffset; k++) {

                            diffLeft.push({ type: 'empty', text: '', line: '' });

                            diffRight.push({ type: 'added', text: escapeHtml(currLines[j]), line: rightLineNum++ });

                            j++;

                        }

                    } else if (removedOffset !== -1) {

                        for(let k=0; k<removedOffset; k++) {

                            diffLeft.push({ type: 'removed', text: escapeHtml(prevLines[i]), line: leftLineNum++ });

                            diffRight.push({ type: 'empty', text: '', line: '' });

                            i++;

                        }

                    } else {

                        if (pLine !== null) {

                            diffLeft.push({ type: 'removed', text: escapeHtml(pLine), line: leftLineNum++ });

                            i++;

                        } else {

                            diffLeft.push({ type: 'empty', text: '', line: '' });

                        }

                       

                        if (cLine !== null) {

                            diffRight.push({ type: 'added', text: escapeHtml(cLine), line: rightLineNum++ });

                            j++;

                        } else {

                            diffRight.push({ type: 'empty', text: '', line: '' });

                        }

                    }

                }

            }

            return { diffLeft, diffRight };

        }

 

        function calculateScore(html, textLength) {

            let score = 5;

            if (textLength > 50) score += 2;

            if (html.includes('<h1>') || html.includes('<h2>')) score += 1;

            if (html.includes('<p>')) score += 1;

            if (html.match(/<[^>]+style="[^"]+"/)) score -= 1;

            return Math.min(Math.max(score, 0), 10);

        }

 

        function analyzeChanges() {

            const prevRaw = document.getElementById('prev-code').value;

            const currRaw = document.getElementById('curr-code').value;

 

            document.getElementById('results').classList.add('hidden');

            document.getElementById('loading').classList.remove('hidden');

 

            setTimeout(() => {

                // 1. Content Stats

                const prevText = stripHtmlTags(prevRaw);

                const currText = stripHtmlTags(currRaw);

                const prevWords = getWordCount(prevText);

                const currWords = getWordCount(currText);

                const diffWords = currWords - prevWords;

 

                document.getElementById('stat-prev-words').textContent = prevWords;

                document.getElementById('stat-curr-words').textContent = currWords;

               

                const addedEst = diffWords > 0 ? diffWords + Math.floor(prevWords * 0.05) : Math.floor(currWords * 0.05);

                const removedEst = diffWords < 0 ? Math.abs(diffWords) + Math.floor(currWords * 0.05) : Math.floor(prevWords * 0.05);

               

                document.getElementById('stat-added').textContent = `+${addedEst}`;

                document.getElementById('stat-removed').textContent = `-${removedEst}`;

 

                // 2. Observations

                const observations = analyzeObservations(prevRaw, currRaw);

                const obsList = document.getElementById('observations-list');

                obsList.innerHTML = '';

                observations.forEach(obs => {

                    const li = document.createElement('li');

                    li.innerHTML = `<i data-lucide="${obs.icon}" style="width: 20px; height: 20px; color: ${obs.color}; flex-shrink: 0; margin-top: 2px;"></i> <span>${obs.text}</span>`;

                    obsList.appendChild(li);

                });

 

                // 3. Smart Summary

                const actionText = diffWords > 0 ? "expanded" : (diffWords < 0 ? "reduced" : "modified");

                const summaryList = document.getElementById('smart-summary-list');

                const scorePrev = calculateScore(prevRaw, prevWords);

                const scoreCurr = calculateScore(currRaw, currWords);

 

                summaryList.innerHTML = `

                    <li>The document has been <span class="font-bold">${actionText}</span> compared to the previous version.</li>

                    <li>Likely updated to <span style="font-weight: 600;">${observations[0]?.text.toLowerCase() || 'refine the content structure and wording.'}</span></li>

                    <li>Overall structure and readability score is now <span class="font-bold">${scoreCurr}/10</span>.</li>

                `;

 

                // 4. Metadata

                const metaPrev = extractMetadata(prevRaw);

                const metaCurr = extractMetadata(currRaw);

                document.getElementById('meta-author').textContent = metaCurr.author !== "Not available" ? metaCurr.author : metaPrev.author;

                document.getElementById('meta-modified').textContent = metaCurr.modifiedBy !== "Not available" ? metaCurr.modifiedBy : metaPrev.modifiedBy;

 

                // 5. Scoring

                document.getElementById('score-prev').textContent = `${scorePrev}/10`;

                document.getElementById('score-curr').textContent = `${scoreCurr}/10`;

               

                setTimeout(() => {

                    document.getElementById('score-bar-prev').style.width = `${scorePrev * 10}%`;

                    document.getElementById('score-bar-curr').style.width = `${scoreCurr * 10}%`;

                }, 100);

 

                let recommendation = "Content looks solid. Ensure HTML semantics are maintained.";

                if (scoreCurr < 6) recommendation = "Consider using semantic HTML tags (h1, h2, p) to improve structure and readability.";

                else if (scoreCurr > scorePrev) recommendation = "Great job! The structural quality has improved in this version.";

                document.getElementById('score-recommendation').textContent = recommendation;

 

                // 6. Detailed Diff View using internal CSS classes

                const prevLines = prevRaw.split('\n');

                const currLines = currRaw.split('\n');

                const diffData = computeLineDiff(prevLines, currLines);

               

                const container = document.getElementById('diff-unified-container');

                let diffHtml = '';

 

                for(let i = 0; i < diffData.diffLeft.length; i++) {

                    const left = diffData.diffLeft[i];

                    const right = diffData.diffRight[i];

 

                    let leftNumClass = 'h-norm-num';

                    let leftContentClass = '';

                    if (left.type === 'removed') { leftNumClass = 'h-rem-num'; leftContentClass = 'h-rem-txt'; }

                    else if (left.type === 'empty') { leftNumClass = 'h-empty-num'; leftContentClass = 'h-empty-txt'; }

 

                    let rightNumClass = 'h-norm-num';

                    let rightContentClass = '';

                    if (right.type === 'added') { rightNumClass = 'h-added-num'; rightContentClass = 'h-added-txt'; }

                    else if (right.type === 'empty') { rightNumClass = 'h-empty-num'; rightContentClass = 'h-empty-txt'; }

 

                    diffHtml += `

                    <div class="diff-row">

                        <!-- Left Split -->

                        <div class="diff-half">

                            <div class="d-num ${leftNumClass}">${left.line || ''}</div>

                            <div class="d-text ${leftContentClass}">${left.text || ' '}</div>

                        </div>

                        <!-- Right Split -->

                        <div class="diff-half">

                            <div class="d-num ${rightNumClass}">${right.line || ''}</div>

                            <div class="d-text ${rightContentClass}">${right.text || ' '}</div>

                        </div>

                    </div>

                    `;

                }

               

                container.innerHTML = diffHtml;

                lucide.createIcons();

               

                document.getElementById('loading').classList.add('hidden');

                document.getElementById('results').classList.remove('hidden');

               

            }, 800);

        }

    </script>

</body>

</html>

 
