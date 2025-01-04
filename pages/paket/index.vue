<template>
  <SubHeader>
    <div>
      <Button label="Tambah" size="small" severity="success" @click="modalPaket({})" class="w-full" >
        <Icon name="lucide:plus" size="1em" /> Tambah Paket
      </Button>
    </div>
  </SubHeader>
  <DataTable :value="data" striped-rows tableStyle="min-width: 50rem">
      <Column field="id_paket" header="ID"></Column>
      <Column field="paket" header="Paket"></Column>
      <Column field="bobot" header="Bobot"></Column>
      <Column header="Tindakan">
          <template #body="slotProps" >
            <div class="flex justify-center gap-1">
              <Button severity="success" size="small" @click="modalPaket(slotProps.data)">
                  <Icon name="lucide:edit" size="1em" />
              </Button>
              <Button severity="danger" size="small" @click="deletePaket(slotProps.data)">
                  <Icon name="lucide:trash" size="1em" />
              </Button>
            </div>
          </template>
      </Column>
  </DataTable>
  <ConfirmPopup>
    <template #container="{ message, acceptCallback, rejectCallback }">
      <div class="rounded p-4">
        <span>{{ message.message }}</span>
        <div class="flex items-center gap-2 mt-4">
            <Button label="Hapus" severity="danger" @click="acceptCallback" size="small"></Button>
            <Button label="Batal" severity="contrast" @click="rejectCallback" size="small"></Button>
        </div>
      </div>
    </template>
  </ConfirmPopup>
  <Toast />
  <DynamicDialog />
  <div>
    <div class="text-red-500">{{ data }}</div>
  </div>  
</template>

<script lang="ts" setup>
  definePageMeta({
      title: "Paket",
  });
  const client = useSanctumClient()
  const confirm = useConfirm()
  const toast = useToast()
  const dialog = useDialog()
  const ModalPaket = defineAsyncComponent(() => import('~/components/ModalPaket.vue'))
  const { data, error, refresh } = await useAsyncData('paket', fetchData)
  function fetchData() {
    return client(`/api/paket`);
  }

  const deletePaket = async (paket: any) => {
    confirm.require({
      message: 'Apakah Anda yakin ingin menghapus paket ini?',
      header: 'Konfirmasi',
      icon: 'lucide:alert-triangle',
      rejectProps: {
        label: 'Batal',
        severity: 'secondary',
        outlined: true
      },
      acceptProps: {
        label: 'Hapus'
      },
      accept: async () => {
        await client(`/api/paket/${paket.id_paket}`, {
          method: 'DELETE',
        });
        toast.add({ severity: 'success', summary: 'Success', detail: 'Delete konsumen berhasil!', life: 3000 });
        refresh();
      },
    })
  }
  const modalPaket = (data: any) => {
    dialog.open(ModalPaket, {
      data: data,
      props: {
        header: `${data.paket}`,
        dismissableMask: true,
        dismissable: true,
        showHeader: false,
        class: 'w-full max-w-[500px]',
        modal: true
      } as any,
      emits: {
        refreshData: () => {
          console.log('enit refresh');
          refresh()
          toast.add({ severity: 'success', summary: 'Success', detail: 'Paket berhasil disimpan!', life: 3000 })
        }
      }
    });
  }
</script>

