<script>
  export let msg;
  export let acao;
  export let id = "alertaModal"; 
</script>

<div class="modal fade" id={id} tabindex="-1" aria-labelledby="modalLabel" aria-hidden="true">
  <div class="modal-dialog modal-dialog-centered">
    <div class="modal-content">
      <div class="modal-body">
        {msg}
      </div>
      <div class="modal-footer">
        <button class="btn btn-danger" data-bs-dismiss="modal" onclick={acao}>Sim</button>
        <button class="btn btn-primary" data-bs-dismiss="modal">Cancelar</button>
      </div>
    </div>
  </div>
</div>
