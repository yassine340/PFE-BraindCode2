<template>
    <AuthenticatedLayout>
      <div class="container mx-auto p-6">
        <h1 class="text-4xl font-bold text-gray-800 mb-6 text-center">✏️ Modifier la formation</h1>
  
        <form @submit.prevent="updateFormation" class="bg-gray-100 p-6 rounded-lg shadow-lg">
          <!-- Titre -->
          <label class="block font-semibold">Titre</label>
          <input v-model="form.titre" type="text" class="w-full p-2 border rounded mb-4">
  
          <!-- Prix -->
          <label class="block font-semibold">Prix (€)</label>
          <input v-model="form.prix" type="number" class="w-full p-2 border rounded mb-4">
  
          <!-- Est Certifiante -->
          <label class="block font-semibold">Certification</label>
          <select v-model="form.estcertifiante" class="w-full p-2 border rounded mb-4">
            <option :value="true">Oui</option>
            <option :value="false">Non</option>
          </select>
  
          <!-- Image -->
          <label class="block font-semibold">Image</label>
          <input type="file" @change="handleImageUpload" class="w-full p-2 border rounded mb-4">
  
          <!-- Boutons -->
          <div class="flex justify-between mt-4">
            <button type="submit"
              class="px-6 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition duration-200">
              💾 Sauvegarder
            </button>
            <Link :href="`/formations/${formation.id}`"
              class="px-6 py-2 bg-gray-400 text-white font-semibold rounded-lg hover:bg-gray-500 transition duration-200">
              Annuler
            </Link>
          </div>
        </form>
      </div>
    </AuthenticatedLayout>
  </template>
  
  <script setup lang="ts">
  import { ref } from "vue";
  import { useForm, router } from "@inertiajs/vue3";
  import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
  
  interface Formation {
    id: number;
    titre: string;
    prix: number;
    estcertifiante: boolean;
    image_formation: string | null;
  }
  
  // Props depuis Laravel
  const props = defineProps<{ formation: Formation }>();
  
  // Création du formulaire avec Inertia.js
  const form = useForm({
    titre: props.formation.titre,
    prix: props.formation.prix,
    estcertifiante: props.formation.estcertifiante,
    image_formation: null as File | null
  });
  
  // Gérer le téléchargement de l'image
  const handleImageUpload = (event: Event) => {
    const file = (event.target as HTMLInputElement).files?.[0];
    if (file) form.image_formation = file;
  };
  
  // Fonction pour mettre à jour la formation
  const updateFormation = () => {
    router.post(`/formations/${props.formation.id}`, {
      _method: "PUT",
      titre: form.titre,
      prix: form.prix,
      estcertifiante: form.estcertifiante,
      image_formation: form.image_formation
    }, {
      onSuccess: () => alert("Formation mise à jour avec succès !")
    });
  };
  </script>
  