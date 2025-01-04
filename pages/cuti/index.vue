<template>
  <SubHeader>
    <div>
      <Button label="Tambah" size="small" severity="success" @click="modalCuti({})" class="w-full" >
        <Icon name="lucide:plus" size="1em" /> Tambah Cuti
      </Button>
    </div>
  </SubHeader>

  <DataTable :value="data" paginator :rows="10" :rowsPerPageOptions="[5, 10, 20, 50]" striped-rows tableStyle="min-width: 50rem">
      <Column field="id" header="ID"></Column>
      <Column field="nama" header="Nama"></Column>
      <Column field="tanggal" header="Tanggal"></Column>
      <Column field="jenis" header="Jenis"></Column>
      <Column field="detail" header="Detail"></Column>
      <Column header="Tindakan">
          <template #body="slotProps" >
            <div class="flex justify-center gap-1">
              <Button severity="success" size="small" @click="modalCuti(slotProps.data)">
                  <Icon name="lucide:edit" size="1em" />
              </Button>
              <Button severity="danger" size="small" @click="deleteCuti(slotProps.data)">
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
      title: "Cuti",
  });
  const client = useSanctumClient()
  const confirm = useConfirm()
  const toast = useToast()
  const dialog = useDialog()
  const ModalCuti = defineAsyncComponent(() => import('~/components/ModalCuti.vue'))
  const { data, error, refresh } = await useAsyncData('cuti', fetchData)

  function fetchData() {
    return client(`/api/cuti`);
  }

  const deleteCuti = async (cuti: any) => {
    confirm.require({
      message: 'Apakah Anda yakin ingin menghapus data cuti ini?',
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
        await client(`/api/cuti/${cuti.id}`, {
          method: 'DELETE',
        });
        toast.add({ severity: 'success', summary: 'Success', detail: 'Delete data cuti berhasil!', life: 3000 });
        refresh();
      },
    })
  }
  const modalCuti = (data: any) => {
    dialog.open(ModalCuti, {
      data: data,
      props: {
        header: `${data.cuti}`,
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
          toast.add({ severity: 'success', summary: 'Success', detail: 'Data Cuti berhasil disimpan!', life: 3000 })
        }
      }
    });
  }
</script>

