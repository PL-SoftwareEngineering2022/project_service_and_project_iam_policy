data "google_project" "project" {}
locals {
  enable_apis = {
    "artifactregistry" : "artifactregistry.googleapis.com",
    "bigtableadmin" : "bigtableadmin.googleapis.com"
    "cloudbuild" : "cloudbuild.googleapis.com",
    "cloudfunctions" : "cloudfunctions.googleapis.com",
    "cloudresourcemanager" : "cloudresourcemanager.googleapis.com",
    "compute" : "compute.googleapis.com",
    "container" : "container.googleapis.com",
    "dns" : "dns.googleapis.com",
    "firestore" : "firestore.googleapis.com",
    "iam" : "iam.googleapis.com",
    "iamcredentials" : "iamcredentials.googleapis.com",
    "cloudkms" : "cloudkms.googleapis.com",
    "secretmanager" : "secretmanager.googleapis.com",
    "securitycenter" : "securitycenter.googleapis.com",
    "servicecontrol" : "servicecontrol.googleapis.com",
    "servicenetworking" : "servicenetworking.googleapis.com",
    "servicemanagement" : "servicemanagement.googleapis.com",
    "serviceusage" : "serviceusage.googleapis.com",
    "spanner" : "spanner.googleapis.com",
    "cloudsql" : "sqladmin.googleapis.com",
  }
  service_agents_policy = {
    "roles/artifactregistry.serviceAgent"  = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-artifactregistry.iam.gserviceaccount.com"],
    "roles/cloudbuild.serviceAgent"        = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-cloudbuild.iam.gserviceaccount.com"],
    "roles/cloudfunctions.serviceAgent"    = ["serviceAccount:service-${data.google_project.project.number}@gcf-admin-robot.iam.gserviceaccount.com"],
    "roles/compute.serviceAgent"           = ["serviceAccount:service-${data.google_project.project.number}@compute-system.iam.gserviceaccount.com"],
    "roles/container.serviceAgent"         = ["serviceAccount:service-${data.google_project.project.number}@container-engine-robot.iam.gserviceaccount.com"],
    "roles/firestore.serviceAgent"         = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-firestore.iam.gserviceaccount.com"],
    "roles/cloudkms.serviceAgent"          = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-cloudkms.iam.gserviceaccount.com"],
    "roles/servicenetworking.serviceAgent" = ["serviceAccount:service-${data.google_project.project.number}@service-networking.iam.gserviceaccount.com"],
    "roles/spanner.serviceAgent"           = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-spanner.iam.gserviceaccount.com"],
    "roles/cloudsql.serviceAgent"          = ["serviceAccount:service-${data.google_project.project.number}@gcp-sa-cloud-sql.iam.gserviceaccount.com"],
  }
}

resource "google_project_service" "enable_apis" {
  project  = var.GCP_PROJECT
  for_each = local.enable_apis
  service  = each.value
}

resource "google_project_iam_policy" "common_service_agents_policy" {
  project     = var.GCP_PROJECT
  policy_data = data.google_iam_policy.service_agents_policy.policy_data
}

data "google_iam_policy" "service_agents_policy" {
  dynamic "binding" {
    for_each = concat(keys(local.service_agents_policy))
    content {
      members = compact(
        concat(
          lookup(local.service_agents_policy, binding.value, [""])
        )
      )
      role = binding.value
    }
  }
}
