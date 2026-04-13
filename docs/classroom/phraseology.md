## Phraseology References

<div class="pdf-tabs">
  <div class="tab-buttons">
    <button class="tab-btn active" onclick="showTab('doc1')">Doc 9432</button>
    <button class="tab-btn" onclick="showTab('doc2')">Doc 4444</button>
  </div>
  <div id="doc1" class="tab-content active">
    <iframe src="../../assets/pdfs/Doc9432.pdf" width="100%" height="600px"></iframe>
  </div>
  <div id="doc2" class="tab-content" style="display:none">
    <iframe src="../../assets/pdfs/Doc4444.pdf" width="100%" height="600px"></iframe>
  </div>
</div>

<script>
function showTab(id) {
  document.querySelectorAll('.tab-content').forEach(el => el.style.display = 'none');
  document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
  document.getElementById(id).style.display = 'block';
  event.target.classList.add('active');
}
</script>